---
layout: post
title:      "Entering a while loop"
date:       2019-12-09 19:00:45 -0500
permalink:  entering_a_while_loop
---

At the end of the first month, I have finished my CLI Project. It works 100%. The only thing left to do is to colorize it and maybe add some other fancy things. In the characters section of my CLI Menu, I have to enter a while loop so that the user doesn't get stuck in my program as it loops on for eternity. Here is my code:



def characters
         puts Scraper.all_characters
         input = " "
         while input != '0'
         puts ""
         puts "Hit a number from 1-10 to see a character bio!"
         puts "If you would like to go back to the menu, type '0' or hit enter!"
         input = gets.strip.to_i

            if (1..Scraper.bios.length).include?(input)
                bio = Scraper.bios[input - 1]
                puts "#{bio}"
            elsif input < 1 || input > 10
                break
            end
        end
    end
```


So what is happening here is that my characters method will now put out a list of characters from 'Scraper.all_characters', then it will say the input is equal to a string. 

While that input is not equal to 0, my loop will run. Now the user will be able to choose a character from 1 to 10, and they can also repeatedly view characters. 

If their input is less than 1 or if it is greater than 10, it will 'break' the loop and return to the main menu. Also if the user decides to hit any other random key, it will return to the main menu as well.



