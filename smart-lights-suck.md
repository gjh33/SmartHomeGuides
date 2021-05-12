# Smart Light Bulbs Suck
The purpose of this guide is to talk about my experience with various smart lights and particularily what I see missing from the many reviews I looked over before purchasing mine. I went entirely with wifi bulbs for my setups, however what I talk about will apply to any bulb. I won't be talking about the different platforms, but more what i've learned to look for in a good bulb. I will be focusing on my experience with the 3 types of bulbs I have purchased: Teckin, Yeelight 1s, Lifx a19

## You Get What You Pay For
This may be the hardest one for people to hear, but it's true. I started my smart home with one google speaker and 10 Teckin RGBW Bulbs. They were incredibly cheap, worked relatively well, and were a great entry point into smart lighting. I didn't really realize what I was missing, but I saw some fellow youtubers rate them pretty low. The colors were also not very bright, so it was hard to fill a room with a hue. Queue Yeelight. I'd seen for quite some time Yeelight toted as the hue killer. People boasted Yeelight had great saturation of colors, great white colors, and all in all performed on par with hue for half the price. Well if you've read / seen the same things I did let me stop you here. It's bullshit. While Yeelight is a significant step up from Teckin, I actually lost several features. Red, Blue, and Green were incredibly bright and saturated. Some colors like magenta also performed well. Where the Yeelight absolutely fell flat on it's ass was with warm colors. Orange is non existent. It looks more like a light peach. Turning the white values into the warmer side produces green / pinkish hues. The bulb's white values hold strong from 0 - 300 mireds. It's daylight color at 200 mireds is very bright and holds against any expensive bulbs. However after 300 the white colors just look off. They appear faded, they get dimmer, and the color accuracy is off. If you set the bulbs to "orange" via google or any other service, the lights appear green. If you tweak it manually you can eventually find an orangish color, but it's closer to beige or peach. It's completely unsaturated. Lastly, I recently tried Lifx. I bought 1 and justified the cost to see if they are really worth the price. My god are they ever. They are bright! The colors are more saturated and perfect than any other smart bulb. The orange color is rich and saturated as well. They can produce a yellow light seemlessly, which is even hard than orange. Google's definition of "orange" doesn't work great on Lifx, and sometimes you have to manually find the right rgb values to produce orange. However it is comparitively amazing. The white colors are stunning and bright at all levels. They fill a room and blow all the other lights out of the water. Yeelight looks closer to Teckin when Lifx is put next to it. Teckin on the other hand while it says it can go warm, won't go anywhere near yeelight and Lifx. And Lifx wins with the warmest colors. Lifx isn't perfect thought (but damn near it). First, home assistant didn't detect Lifx's true max mired tempurature, meaning I had to add a customization to the bulb to get those warmer colors from home assistant. However, Lifx has some other bonuses. Lifx is the only bulb in my house where the "transition" value in home assistant actually works. Furthermore, it has a "set state" service call that allows you to change the bulb settings without turning it on. This is a GAME CHANGER. Overall I'm blown away by Lifx. The one consistent theme with these 3 bulbs was, the more I payed the better the bulb. I've seen videos of phillips hue doing orange. I've heard they're not as bright as Lifx, but they are SLIGHTLY cheaper after a point. The upfront cost of a hub will offset that to the point it would take several rounds of hue lights to justify the cost. As a fan of wifi bulbs, Lifx is a clear winner. I've had no connection issues running 40+ wifi bulbs and 20+ other wifi devices. I use a gaming router (nighthawk). I bought this router with the processor in mind years ago because I wanted no compromises gaming wifi for when friends came over. As long as you have a good router with a good dual core processor, you shouldn't have any issues. This isn't a review on routers, but just know processor specs tend to be hard to find. Often they only care about their LAN transfer speeds which no one gives a shit about. It's just a big number to wave in our faces.

## The Hunt For Orange And Yellow
This is something that drives me crazy when looking into smart lighting. Almost every smart light out there has decent red, green, and blue colors. These aren't always the same brightness or consistency, but if you're looking for those colors you're probably happy. Where bulbs start to struggle is with color rendering of mixed colors. All the intermediary colors are where bulbs start to differentiate. Most bulbs handle cyan and magenta really well. I think this is because those are the kinds of colors people expect with RGB bulbs. Purple is a very average color. I find some bulbs have trouble reproducing the specific RGB value, but if you play with it enough any bulb can get a good purple. What I mean by this is if you input (255, 0, 255) you might not get a perfect purple. But that perfect purple DOES exist on that bulb, you just have to put in the "wrong" values to find it. I'm going to refer to this ability to reproduce rgb values as rendering accuracy. This is less important for ambient lighting, but very important for effect lighting (like a tv backlight). Now when it comes to the colors of orange and yellow, I have yet to find a smart bulb that is perfect. I've found smart rgb strips, but not bulbs. Lifx can produce every color perfectly, but if you ask google for yellow, or input a yellow rgb value, you won't get a perfect yellow. It's by far the closest, but to get that perfect yellow there's a very tiny sweet spot in the color wheel. Orange's sweet spot is much bigger and Lifx produces orange really well. One important thing to note, google is the worst at getting colors right. Many bulbs if you as for pink, you get a cyan / teal color. You have to ask for magenta, hot pink, or dark pink. But I digress. Orange and yellow are the two colors that every bulb consistently has the most trouble rendering accurately. And many bulbs can't even produce those shades at all. Teckin has a reasonable orange, and non existant yellow. In similar fasion, many of it's colors follow that trend. Teckin has terrible rendering accuracy, but can produce most of the colors with enough tweaking. Yeelight is by far the most inconsistent. Where it's better than teckin it's way better, and where it's worse than teckin it's way worse. But again orange and yellow are the bit lackers here. Yeelight is completely incapable of either. And what infuriates me more is I put in tons of time and effort into researching these bulbs and not one video talks about orange or yellow. Even some of the best videos out there (Smart Home Solver is the best for comparisons) has no mention of this. After Yeelight I thought maybe all bulbs are just incapable. But then I see streamers with their hue bulbs that rotate without issues through and orange color. I also purchased an RGB strip that has perfect orange and yellows, and very very very good rendering accuracy. Smart lights suck. In the end, this is just here for you all to be aware. Try your best to see how your lights render orange or yellow. Since smart lights are RGBW, the warmer whites are produced by mixing in an orange hue. If your lights can't produce orange or yellow well, your warm whites will be off too. I leave this information here in hopes others will start to review the orange and yellow colors on a smart bulb, and that you as a consumer will demand this information as well. My rankings if not already obvious, LIFX > Teckin > Yeelight. I would however rate yeelight overall better, for many reasons. For one, yeelight's warm whites are as good as teckin until past 300. But teckin's bulbs while toating up to 400, never really go past 300. So yeelight only starts shitting the bed where teckin can't go anyways. The orange on teckin is useful for night lights, but yeelight can also get very dim, even if the orange colors are off.

## RGB Strips Are Way Better
This is where I go more from advice, to ranting. It astounds me just how much better RGB strips are than RGB smart lights. Let me explain. For those who don't know how these lights work, a basic RGB light strip will have small red, blue, and green lights. These lights change brightness and can mix together to render various RGB colors. With theoretically perfect controllers and perfectly manufactured LED diodes that never degrade, you COULD render all the white colors as well with just RGB. However in reality, RGB lights just aren't consistent enough. White requires a perfect balance, and humans distinguish it really easilly. Take any rgb light you have and try to make it white using the RGB mode. It will almost always be an off hue of either blue, green, or red. That's where white LEDs come in. Some strips, and all rgb white bulbs use what's called RGBW configuration. Meaning each LED has 4 diodes. Red, green, blue and an additional white. It's up to the manufacturer what tempurature this white is. If it's on the warmer spectrum you'll see it call RGBWW for "Warm White". If it's on the cooler side you'll see RGBCW for "Cool White". The way you then produce alternate hues is by mixing in rgb. If you have a close to pure white LED, you can mix in some blue for cooler light, and mix in some amber (orange) for a warmer light. Are you starting to see why the above section was important? Orange matters! Now, understandably we have a similar problem. Orange is pretty hard to render (apparently because only my LIFX bulb can do it). RGB strips have a solution to this. What if we had a neutral white, and an extremely warm white light, and we just blend between those two. This is genious. My first RGB strip ever was like this and it's called RGBCCT. I have yet to find a SINGLE smart bulb that's RGBCCT. Even Lifx advertises as RGBW. WHY?!?! This seems like a no brainer. RGB Strips have had it for a few years now. My guess is space constraints? But we've seen these RGBCCT on pretty small LED chips in rgb strips. Smart lights are just behind. The first ever RGB strip I bought has by far the most ACCURATELY rendered colors of any rgb light in my house. The whites are perfect. The colors are perfect. I can pick any shade of orange in home assistant and get that EXACT color out of those lights. Can you say that for any smart bulb in your home? Hue users let me know. But Lifx still isn't quite there. You can achieve those colors, but you have to mess around on the color wheel to be perfect. They get close, but not as close as my RGBCCT strip. Now for those interested, I have the 5050 60LEDs/m 5m RGBCCT strip from BTF-Lighting. I use some random tuya compatible controller since my setup is still wifi based. My point in all this, is it's so freakin' easy to just find a good color accurate rgb strip. I did my research, I googled around, and my first purchase hit the mark 100%. Comparitively, I'd been through 3 light brands to find only the insanely expensive lifx even capable of approaching the standards of my rgb strip. RGB Strips are just better, and smart lights need to get their shit together! If you can light your room with rgb strips, go for it! You'll probably have much more success in getting what you want. Cheaper too. And damn are they bright. My strips consistently overpower my yeelights. I have 2 yeelights in my ceiling in one fixture, and the 5m strip on my floor. The strip obliterates the yeelights.