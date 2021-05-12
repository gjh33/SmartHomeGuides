# Making An Easy To Maintain Smart Home With Home Assistant
As a professional programmer, something that's very important when building any system that is subject to change is to make it easy to change. Home Assistant by default does a terrible job of this and I found no other guides on this topic, leaving me to figure it out for myself. Let's start with talking about what makes a system easy to maintain. For my fellow programmers out there, you can skip this next section.

## What Makes A Maintainable System
While programming has many ways to define a maintainable system, Home Assistant comes down to two principles. While I can't teach you every single trick to make your home maintainable, I can give you some goals that will lead you in the right direction. Home Assistant isn't well equipped for these principles, and at the end I'll talk about some features that could help.

The first principle is "decoupling". Decoupling simply means that something in your smart home, shouldn't rely on too many other things in your smart home. For example, if you remove a light bulb from your home, and have to then modify or remove 50 automations, that's a very high coupling. If I replace a Yeelight with a LIFX bulb and all my automations break, that's a high coupling. Conversely my smart home is decoupled if changing out a light bulb, only requires editing one config file (yes, that is the goal we will achieve).

The second principle is "D.R.Y." which stands for "Don't Repeat Yourself". This principle is all about minimizing copy and paste. For example, let's say we want to enable our google speakers to tell us when our food order is on the way for a given room in our house. We could make an automation for each room, that is essentially a copy paste, then toggle those automations on/off per room. Or we could make one automation that uses helpers to toggle which speakers get notified. If we swap out a speaker for a new one, the first solution would require us to go into each automation and update that entity. The latter would require us to only update one automation. Note that in both scenarios, this automation is coupled to the speakers. It relies on those speakers and if the speakers change we have to change the automations. We can do better here too, and we'll cover that later.

## Entity Aliases
One of my first major annoyances was whenever I had to replace my cheap teckin bulbs. Swapping out a bulb involved going into the Smart Life app, adding the new bulb, deleting the old bulb, syncing my google home (we'll get there later), reloading my home assistant integration, going over and finding all automations and UI that referenced that bulb and updating those references to the new bulb. It was a HUGE pain just to change a lightbulb. This is not the smart home I was promised. So I thought, well how do we do this in programming. We'd make a variable. It doesn't matter what the value of that variable is, as long as it's the right type of thing, we can use that variable name in our code. The same principle applies to smart homes. Wouldn't it be great, if we could define a name for a light, and then tell home assistant what physical entity that light is right now? Then in all our automation we can reference that name. If we were to swap out the physical bulb, we just tell home assistant that name is now pointing to a different bulb. All our automations use the name, so we only had to change one reference! Well we can do exactly this. The first thought was "man wouldn't it be great if home assistant had aliases?". Well tough. It doesn't. But i has a few features that act as aliases.

If you're able to define your entity id without breaking your integrations, then you could just do that. This gets annoying though when swapping bulbs. Maybe I want that LIFX bulb in the bathroom now, and to move my bathroom Yeelight into the living room where the LIFX was. It's do able, but now you have to edit existing entity IDs which is much harder than just changing the ID when you add it. OK. So what else you got?

Well what about template entities? Yes. These will work. For sure. You can make a template light for every light in your home and evaluate the underlying physical light's attributes. But come on! This is a HUGE pain. And templates aren't easy for everyone. They're an advanced feature, and unless you want something more than an alias, you can do better. Templates would make sense if you wanted all lights to support the same effect (like "rotate color") but each physical light has different effect names that you can use to do that. A template light is a great way to solve that problem.

Ok, so what can we do that's simple, fast, and effective. Light Groups. Light groups show up like any other light entity. They can be given names. And their features are a superset of their child lights. So, just make a light group with a single physical bulb. In all your automations / other light groups, etc you can reference this alias group. Now if I swap out my physical bulb, I only have to edit the config file with my alias group. I simply change the single entity in that group and BAM! All other groups / automations / love lace UI / Google home all still work! Well what about things other than lights? Well many group types exist. Stay as specific as possible. For example, putting lights into a normal group will lose the ability to have them be controlled as a light in your lovelace UI. But putting them into a Light Group makes them still show up as a light. For switches, normal groups work fine. For speakers, universal media players act as a group, and for notifications use notification groups. That way if you get a new phone, just replace the phone entity in your notification group. Most things in Home Assistant can go into groups of some kind, however not everything will and still maintain it's functionality. For most of these cases you can use template entities. And of course not everything needs to be given an alias. If you only change out your phone every 4 years, maybe it's not a big deal to swap out every reference. Maybe it's not worth the upfront cost of creating all the groups.

Here's an example of how one could setup their Home Assistant instance for aliases. Go over to your configuration.yaml file. Yes you need to use YAML for this. Yet another complain I have for home assistant, but this isn't too bad. I highly recommend getting the visual studio code extension, since it will make finding entity ids much easier with it's autocomplete. Normally to make say a light group, you would have something like this:

```yaml
light:
    - platform: group
      name: MyGroupName
      entities:
        - light.abc123
        - light.def456
```

Ok, first step here is to abstract this into a different file. We don't want our configuration.yaml to get cluttered by all these groups and aliases.

Make a new file in the same folder as your configuration.yaml. Call it light-groups.yaml. Now in your configuration.yaml change the code to the following:

```yaml
light: !include light-groups.yaml
```

and in your light-groups.yaml file put all the text normally under the "light" key.

```yaml
- platform: group
    name: MyGroupName
    entities:
    - light.abc123
    - light.def456
```

The way the `!include` works is it just copy pastes what's in the other file. So put in that file exactly what would be directly under your "light" yaml key normally.

Ok we can do one better. Remember, our aliases aren't just groups, but should also be used by other groups. Home Assistant lets us split one config into multiple files. But to do this we need to change the tags slightly. First, make a new file called light-aliases.yaml. Then in your configuration.yaml, change it to the following:

```yaml
light aliases: !include light-aliases.yaml
light groups: !include light-groups.yaml
```

Home assistant only cares about the first word "light". The second word just lets you split your configuration into muliple yaml keys. This works for other things like notify groups and any kind of group. Here's a full example of what your setup might look like now.

**configuration.yaml**
```yaml
light aliases: !include light-aliases.yaml
light groups: !include light-groups.yaml
```

**light-aliases.yaml**
```yaml
### BEDROOM ###
- platform: group
  name: Bedroom Ceiling Light 1
  entities:
    - light.teckin_bedroom_1

- platform: group
  name: Bedroom Ceiling Light 2
  entities:
    - light.teckin_bedroom_2

### LIVING ROOM ###
- platform: group
  name: Living Room Couch Lamp 1
  entities:
    - light.lifx_bulb_1

- platform: group
  name: TV Backlight Strip 1
  entities:
    - light.rgb_strip_teckin
```

**light-groups.yaml**
```yaml
- platform: group
  name: Bedroom Lights
  entities:
    - light.bedroom_ceiling_light_1 # The entity ID of our "group" we made in aliases
    - light.bedroom_ceiling_light_2

- platform: group
  name: Living Room Lights
  entities:
    - light.living_room_couch_lamp_1

- platform: group
  name: Living Room Effect Lighting
  entities:
    - light.tv_backlight_strip_1

- platform: group
  name: All Room Lights
  entities:
    - light.bedroom_lights # The entity ID of the group we just made above
    - light.living_room_lights
```

As you can see, nesting groups like this is VERY powerful. Let's say now we want to swap that LIFX bulb with a brand new yeelight (oh god why). Well now we just have to go into light-aliases.yaml, and change the entity under the "Living Room Couch Lamp 1" group. And VOILA!!! All your groups, all your automations referencing the alias, all your love lace UI referencing the alias, will ALL just work magically.

So how should we organize these aliases? When should we make an alias? Should there me multiple aliases for a single entity? Let's start with organization. The simplest is for any light fixtures. While the bulb may change, you will always have those ceiling light fixtures. So this is a great case for an alias. In general, make an alias for the item the bulb would go in. Ok, so what about a couch lamp? We may replace that lamp, or move it to a different location. Now some automations need to be changed, while others may not. This requires some forethought, but in general if your automation is dependent on the fixture itself, or the location of a movable fixture, you should make another alias. Let's take an example for a phillips hue bulb beside the TV in a lamp. We decide to use this bulb in 2 automations. The first, brightens the bulb when watching tv in the dark, to act as a backlight. The second, changes the color of the living room lights in the evening to a warmer color. The first automation depends on the physical location of the device. If we moved the lamp to the other side of the living room, it would no longer be a backlight right? However the second automation would still like to make use of that lamp fixture. It only cares that it's placed in the living room. So for this case we'll make 2 aliases. Even though the second automation depends on it being in the living room, that second automation can reference the "light.living_room_lights" group. If we move the lamp out of the living room, we'd just remove it from that light group and add it to the new location's room group.

```yaml
# This alias is for the fixture itself. Reference this if the automation doesn't care at all where the lamp is
- platform: group
  name: Living Room Ikea Lamp 1 # putting a location in the name isn't the best but it's not the end of the world
  entities:
    - light.phillips_hue_color_white_1

# This alias is for the fixture located beside the tv for backlight use.
# If we move the lamp, this will have to change. Note we use the alias for the fixture, not the bulb directly.
# That way if we sawp out the bulb we still only need to change one spot in the config. It's an alias of an alias
# ALIASCEPTION
- platform: group
  name: TV Backlight Side Lamp
  entities:
    - light.living_room_ikea_lamp_1
```

If you wanted a lamp on each side of the tv, you could imagine instead of a single entity group for the backlight, you could just move it to your light-groups.yaml and make it a normal group called "TV Backlights". It all depends on what automations you want. But your thought should always be: How might I change my devices? How will this impact my automations and UI?

The last thing to talk about is how to use the rest of home assistant to work with these aliases. If you're this deep into automation, and haven't taken manual control of your Lovelace dashboards, then it's about time. In your lovalace dashboard, simply select the alias entity to display on your cards. For automations use entities as much as possible, avoid devices. The modern UI lets you do a lot with entities, and there's very little need to use devices. Use services for actions, use state for conditions and triggers. Now you can reference your alias entities for any automation or script.

That's all there is to know about entity aliasing, and making your smart home easy to refactor. Before commiting to this approach, make sure the group type supports everything you need. Like I said, sometimes your only option is a normal group. These can have reduced functionality. Also consider that not everything is worth this effort. While I could put each speaker in a universal media player, I only have 5 smart speakers and I don't plan to replace them very frequently. They also aren't presently, and don't plan to be used in too many automations. I feel confident in this because no one wants their speakers yelling at them constantly. You may feel differently based on your setup. In the end it's up to you, I'm just sharing my knowledge of how to remedy this problem should it come up.

## D.R.Y Automations
This section is dedicated to maintainable automations. Sometimes you may want to repeat the same automation for multiple areas in your home. And while blueprints are great, what happens if you change the underlying blueprint? Do you have to delete all your automations and re create them with the new blueprint? What a pain! Here's some tips to help you out!

### Avoid Magic Numbers
If any of your automations have number or text in them, consider instead moving that number or text into a helper. Helpers are essentially global variables for Home Assistant. Sadly their support is a bit lacking, and sometimes you have to resort to templating to fetch their values. You may also notice helpers don't have every value type available. How do you input a color? Templates. Remember at any point, you can edit the yaml in your automations, and use templates. Templates output their result as text in the yaml file. Want to represent a color? Have a text helper that contains `255,255,255` or w/e color you want. Then use templates to render that text helper into yaml. 
```yaml
rgb_color: {% raw %}"{{ states('input_text.my_theatre_light_color') }}"{% endraw %}
```

Learning to do this will make your life easier. If the value is only used in one automation, that's fine. But if you have copies of the same automation for each room of your house, and later want to change a value in that automation, it's much nicer if you have one place you can adjust that value.

### Abstract Common Logic Into Events
Consider an automation where you want to have your lights flash when your food delivery has arrived. To do this, you listen to notifications on your phone, and then output to your living room speaker. Each food delivery app sends a different notification, and you need to check for a key phrase in the notification to determine if it's the right one. For example, Uber Eats may use "at the door" and Door Dash may use "has arrived". Rather than have this complex logic duplicated in multiple automation triggers, why not make an automation that fires a custom event? If you notice your trigger logic being repeated across multiple automations, and especially if that logic is subject to change (what if Uber Eats changes their notification text), then consider making a custom event. Put that trigger logic into a new automation, that then fires an event. This event can contain any data that may be useful to your other automations. Now if Uber Eats changes their notification message, you can just change your event automation. Any automations listening to your event remain the same.

### Use Scripts
If you notice a common set of actions repeating across multiple automations that are meant to do the same thing, consider abstracing those actions into a script. You can then call the script from the actions section of your automation. Imagine you want to make your light flash blue, then red, then turn off as a sort of notification. You may have several automations in your home using that sequence. But now you decide you want to add a delay before the first flash blue. Now you have to go into all your automations to add that delay. That's a pain. Instead, if you called a script called "Notification Lights Flash" then instead of fixing all your automations, you would just have to change the one script.

### Helpers To Toggle Automations
Imagine you have an automation that you want to be able to enable or disable for each room in your house. You could make an automation for each room, then expose that automation in your Lovelace UI to be toggled on and off. This has some duplication, but can be pretty maintainable if you use all the methods mentioned above. With one exception. Conditions. Oh Conditions. Now you COULD make a binary template sensor if you have the technical knowledge to do so. But an easier solution is to just combine them into one automation. Instead of having an automation per room, try having one automation. You can use the "Condition Action" and boolean helpers instead. Create a boolean helper for each room, then only execute the action for that room if the boolean helper is toggled on. You can expose this helper in a Lovelace UI. Voila!

## In Summary

While home assistant doesn't easilly support maintainable automations and entities, you've seen there's lots of features we can abuse in the meantime. In general, try to follow the two principles for maintainable smart home systems, and don't go overboard. One day you may move houses, or make other unforeseen changes. Having a smart home is a chore in itself, but what's important is it doesn't become a constant headache. Your life should be made overall easier, and many of these tips will make upkeep much less of a hassle. If you have any other tricks that you think are useful for maintaining a smart home, open an Issue on github and leave a comment. I'll add useful ideas to this page and credit those who suggested them. Now go enjoy your smart home!
