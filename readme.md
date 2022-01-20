codeorg-api         documentation
=================================
###### version 0.0.1
##### Please note API is not currently supported by code.org, and as such no api commands, or examples will work until hostname is approved

codeorg-api is an API made specifically for use in code.org's applab, in order to connect two (or more) clients to one another through a server, 
if you are interested in making any multiplayer or online functionality in applab you've come to the right place! With this documentation you can learn how to get codeorg-api working in your applab project in no time! If you would like to see a program using the codeorg-api you can look [here](https://studio.code.org/projects/applab/yBNFOu6mZku55fLM-Knyw7RjCfnahbN7UVAXi2vt398).

Features
--------

- Pre Specified unique user id's
- Sending and recieving x and y coordinates
- setting a username for the pre specified user id
- getting username
- resetting all values for refreshing server
- getting and setting player ready

Player Settings
---------------
<details><summary>get-user</summary>
  
  <h1>get-user</h1>
  
  <h3>takes 0 params, returns 1 JSON</h3>
  
The first thing you need to do is get your user_id, you can do this using get-user which will return either P1, P2, or higher depending on the server size, which
is pre-set in the server's code, in order to get the server-size changed temporarily for your project contact support. If there are no users available get-user
will then return FULL.

<br>
<br>
URL:
<br>
  <code style="bgcolor=#525252">https://codeorg-api.com/server/get-user</code>
<br>
<br>
Example:
<br>
<code>
  startWebRequest("https://codeorg-api.com/server/get-user", function(content) {
    user = content;
    console.log("completed " + content);
  });
  </code>
</details>

<details><summary>set-username</summary>
  
  <h1>set-username</h1>
  
  <h3>takes 2 params, returns 1 JSON</h3>
  
set-username is the command used to set the username of a specific user id. It uses the user given from get-user, and it also takes another input which is what
the username would be set to, in most cases it would just be whatever the user input in the username/displayname text input but of course usecase can vary
and some people may set-username in their code with no input from the user, depending on what they need. As long as there are no errors calling set-username
will output "set username"

<br>
<br>
URL:
<br>
  <code>https://codeorg-api.com/server/set-username{PLAYER}[USERNAME]</code>
<br>
in the url player would be the user assigned from get-user, for example P1. USERNAME would be the username that is being set, for example what a user would
input in the display-name box.
<br>
<br>
Example:
<br>
<code>
  startWebRequest("https://codeorg-api.com/server/set-username{"+user+"}["+getText("username_input")+"]", function(content) {
    console.log("completed " + content);
  });
  </code>
 <br>
in this example username_input would be the text input of wherever your client is setting their username.
</details>

<details><summary>get-username</summary>
  
  <h1>get-username</h1>
  
  <h3>takes 1 params, returns 1 JSON</h3>
  
get-username is the command used for getting the username of a user ID, currently the server is only set up for two players and when get-username is run
the user from get-user is given as input, and then get-username returns the opposite users username. For example if you were given P1 and you called get-username
you would get P2's username, and if you were given P2 and called get-username you would be given P1's username. If your program requires multiple users
then contact support and we may be able to temporarily change the server properties to fit your needs.

<br>
<br>
URL:
<br>
  <code>https://codeorg-api.com/server/set-username{"+user+"}"</code>
<br>
in the URL user would be the user assigned from get-user
<br>
<br>
Example:
<br>
<code>
  startWebRequest("https://codeorg-api.com/server/set-username{"+user+"}", function(content) {
    other_username = content
  });
 </code>
 <br>
in this example other_username is the variable that you set the output of set-username to that way you can do whatever you need with the other player's username
</details>

<details><summary>set-ready</summary>
  
<h1>set-ready</h1>
  
<h3>takes 1 params, returns 1 JSON</h3>
  
  
The command that sets the property ready to true for specified user id, it takes one input which is the user from get-user, if there are no errors when it is called
  it returns "set to ready"

<br>
<br>
URL:
<br>
  <code>https://codeorg-api.com/server/set-ready{"+user+"}"</code>
<br>
in the URL user would be the user assigned from get-user

<br>
<br>
Example:
<br>
<code>
  startWebRequest("https://codeorg-api.com/server/set-ready{"+user+"}", function(content) {
    console.log("completed " + content);
  });
</code>
</details>

<details><summary>get-ready</summary>
  
  <h1>get-ready</h1>
  
  <h3>takes 1 params, returns 1 JSON</h3>

  
get-ready is the command used to check if a certain user is ready, it takes one input which is the client user's input given from get-user, it then gets the opposit
users ready status and returns that, for example if you are given P1 it would get P2's ready status and return either True or False

<br>
<br>
URL:
<br>
  <code>https://codeorg-api.com/server/get-ready{"+user+"}"</code>
<br>
in the example user would be the user assigned from get-user
<br>
<br>
Example:
<br>
<code>
  startWebRequest("https://codeorg-api.com/server/get-ready{"+user+"}", function(content) {
    other_ready = content
    console.log("completed " + content);
  });
  </code>
<br>
in this example other_ready would be the variable defining if the opposite user is ready or not, then you can use it how you need
</details>

Player Values
-------------
<details><summary>main_command</summary>

  <h1>main_command</h1>
  
  <h3>takes 2 params, returns 1 JSON</h3>


Unlike all the other commands in player settings player values doesn't have a set command, if there is no matching command then the server automatically assumes
that you are trying to get/change player values, also unlike the other commands, in order to make server communication as fast as possible it is a get and post
command all in one. You have two inputs consisting of your user, and a list containing the players x and y coordinates, then the output is the opposite players
x and y, this makes it so rather than making a get and set request (like get-user and set-user or get-ready and set-ready) it can do it in one command, this
is especially important as this command will (most likely) be used to update x and y of the opposite player (depending on use case of course) so by combining
this into one command will decrease the time it takes to make one game loop, and in turn increase the frame rate.

<br>
<br>
URL:
<br>
  <code>https://codeorg-api.com/server/{"+user+"}(x,y)"</code>
<br>
in the example user would be the user assigned from get-user
<br>
<br>
Example:
<br>
<code>
  var player_pos;
  timedLoop(20, function(){
    if(user == "P1"){
      player_pos = "("+getXPosition("P1_img")+","+getYPosition("P1_img")+")";
    }
    else if(user == "P2"){
      player_pos = "("+getXPosition("P2_img")+","+getYPosition("P2_img")+")";
    }
    startWebRequest("https://codeorg-api.com/server/{"+user+"}["+player_pos, function(content) {
      if(user != "P1"){
        setProperty("P2_img","x",parseInt(content.substring("[",",")));
        setProperty("P2_img","y",parseInt(content.substring(",","]")));
      }
      else if(user == "P2"){
        setProperty("P1_img","x",parseInt(content.substring("[",",")));
        setProperty("P1_img","y",parseInt(content.substring(",","]")));
      }
    });
    
  });
</code>
<br>
in example this P1_img and P2_img are the elements representing each player, they don't have to be images, that's just what I decided to use.
</details>
 
Other commands
--------------
<details><summary>reset</summary>
  
  <h1>reset</h1>
  
  <h3>takes 0 params, returns 1 JSON</h3>
  
Command use to reset all data to defaults
<br>
<br>
URL:
<br>
  <code>https://codeorg-api.com/server/reset</code>

<br>
<br>
Example:
<br>
<code>
  startWebRequest("https://codeorg-api.com/server/reset", function(content) {
    console.log("completed " + content);
  });
</code>
</details>

<details><summary>test</summary>
  
  <h1>test</h1>
  
  <h3>takes 0 params, returns 1 JSON</h3>
  
  
Just a command to check that the server is running, and that getting data is working, and all that, returns "Hello world!" on run

<br>
<br>
URL:
<br>
  <code>https://codeorg-api.com/test</code>
<br>
<br>
Example:
<br>
<code>
  startWebRequest("https://codeorg-api.com/test", function(content) {
    console.log("completed " + content);
  });
  </code>
</details> 

Support
-------

If you are having issues, want something to be added, want something in the server to be changed (like player count), or anything else please contact me with:


Email: tyler@spruillmail.com


Phone: (804) 647-5239


Discord: fightingox#9443

Planned Updates
---------------
- Server "Lobbies" so server doesn't have to be resetting, and multiple different programs can run at the same time without interferance
- Unlimited player/object count, adding to array rather than storing as variables
- Saving variables in database so even when server is refreshed previous data can still be accessed (for passwords and stuff like that)
- Upgrading server (currently running server on repl.it planning on moving server to one of my old upgraded PC's with gigabit internet for faster speeds)
- Making applab library to make understanding and using api even easier
- adding security keys so people cant run server commands through web browser

Extra Info
----------

Please let us know of any features that you guys want to be added, or if you need some parameter changed for your project please make us aware of that aswell.
I made this API for a project I had and aside from messing around with the api and multiplayer sometimes I don't have much use for it, however I plan on
keeping the servers running anyway to make it so other people can work multiplayer aswell in there projects. Along with that I don't even have many ideas for
features to add, however I still want to (and plan on) continue to update this api frequently.

Credits
-------

###Made and hosted by Tyler Spruill
