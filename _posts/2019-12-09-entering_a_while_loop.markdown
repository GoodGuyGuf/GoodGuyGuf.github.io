---
layout: post
title:      "One Piece - My CLI Project"
date:       2019-12-09 19:00:45 -0500
permalink:  entering_a_while_loop
---

It is finally done and my programming devil fruit powers have awoken!

Most people I know either do not know about One Piece or are afraid to start watching it.

One Piece is the number 1 best selling manga in the world. It was turned into an anime show in the 90s and is still ongoing with 900+ episodes! It is crazy long. Online, many people have been turned off by its giant episode count. VV

![Imgur](https://i.imgur.com/DURgbCv.png)

So I created a Command Line interface that introduces you to One Piece!

![Imgur](https://i.imgur.com/0QDamJ2.png)

It will summarize what the show is about, show you how many episodes it has currently, give you a list and description of the main protagonists the Straw Hat Pirates, gives you a description of what Devil Fruits are and what Haki is as well as points you to websites that stream the show online.

It was quite a process getting it to work. I created two classes ("Devilfruit", "Character") that were my object classes. That is where I can keep track of every new thing I create. Then I put together a menu, effects and scraper class. The menu would let the user choose the options I give, the effects would put out the colorized logo, and my scraper class uses Nokogiri to grab elements (Headings, body texts) from Wiki pages. It was difficult to connect a scraper class to a menu and making sure everything that I scraped was parsed correctly. The menu would call on scraper to let the user see certain information from a character or devil fruit. Then I had to switch what my menu class was calling on so that it wouldn't connect to my scraper class but to my object classes. That way, my characters have a name and a bio all in one as well as my devil fruit class. Then I could iterate on that object and have it return its name or bio! Then I got to make it all pretty with the gem 'colorize'!

There is so much more that could be added to this CLI. I only included the straw hat pirates, the three types of devil fruits, and what haki is. There are many more heroes and villains that I did not add, as well as a list of all of the devil fruits along with their users, and which users have haki abilities and the ones who do not. Also, I do not include a list or description of its story arcs!

(**SPOILER**)
Here is a scene from my favorite One Piece arc: Enies Lobby -
[Luffy V Blueno](https://youtu.be/wVsT9TPMPlE)
