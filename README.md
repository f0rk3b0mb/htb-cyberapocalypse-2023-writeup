# htb-cyberapocalypse-2023-writeup
hackthebox cyberapocalypse 2023 the cursed mission

## passman

difficulty: easy
category: web

After starting the docker we are greeted with alogin page , we have to register and then login, this challenge comes with downloadable source code.In the docurce we fing out that to get the flag we have to read a note generated by the admin notes as shown below


![screenshot]()



Looking through the source code we find out that the Updatepassword field will be our point of entry(i got a hint from a team mate) as shown below

![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2022-49-30.png)



the username and password will be read from our request so we can use this to cahange the admin password and be able to login as admin. To generate the payload i chatgpteed (haha) , the payload is shown below.

![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2022-51-13.png)



i changed the password of admin to 1234


![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2022-41-07.png)



![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2022-48-47.png)


logging in  the flag is as shown below :)

## didactic otto paddles

difficulty: medium
category: web

This challenge cant deny it was a little tricky, the solution was simple but the enemy was over thinking


When the docker is spawned you are met witha a login page , if you look through the source code you will find the /register page , so lets register a user test we login in and are met by a  online-shop kinda interface


![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-22%2008-41-20.png)


at this point i thought it had to be something about idor on the carts or items in the cart. :)


after alot of struggle we found out that  there was a 'none'  vulnerability in jwt that could  enable us to bypass jwt token signature verification and login as admin, of course by changing uid in jwt data. Credit @Inv1s1bl3.

link to a more detailed explanation [here](https://blog.pentesteracademy.com/hacking-jwt-tokens-the-none-algorithm-67c14bb15771)


so we just have to capture our jwt token and manipulate the uid to 1 for admin and change the alg to 'None' , mind the case since 'none' for some reason doesnt work.

![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-20%2020-47-34.png)


![screenshot]https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-20%2020-47-49.png


replace the cookie in the browser storage with the one above and visit /admin page , it will allow access.

at this point we already had an ssti payload that we could use aganist jsrender on the admin page to cat the flag.txt.



![screenshot]https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-22%2008-54-03.png



{{:"pwnd".toString.constructor.call({},"return global.process.mainModule.constructor._load('child_process').execSync('cat /flag.txt').toString()")()}}


we will register a user using this payload as the username and a random password , then we refresh the admin page 



![screenshot]https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-20%2020-48-03.png 



![screenshot]https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-21%2009-13-18.png



## hijack

difficulty: very easy
category: misc

once you spawn the docker you get ana ip and port , use netcat to connect to the listening socket

![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2018-58-54.png)

it is a simple interface with create config , load config and exit . After playing around with it , i noticed that it generated a string , i tried decoding it using cyberchef as seen below

![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2018-59-54.png)


i had not this kind of syntax before so i googled it and it surns out it was yaml , then i came accross something called yaml deserialization attack.i found a github repo with a python code that can generate serialized yaml payload to perform the attack link is [Here](https://github.com/j0lt-github/python-deserialization-attack-payload-generator).

![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2019-00-23.png)



after generating the payload that does a simple id command i was able to read the flag.txt

![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2019-01-49.png)


![screenshot](https://github.com/f0rk3b0mb/htb-cyberapocalypse-2023-writeup/blob/main/images/Screenshot%20from%202023-03-18%2019-04-19.png)




the flag is shown above :)
