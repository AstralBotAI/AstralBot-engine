# AstralBot

The AstralBot engine is an application to interact and stream a game for an external bot

At the startup a websocket is opened on port 8080 to let a remote bot start a game and interact with it to play.  
The engine send each video frame to the remote bot and your bot should send actions to do.

How To made the remote bot?
===========================

We provide a library to interact with an engine, examples are available on github to begin.

* [python pypi](https://pypi.org/project/astralbot/)

```
pip install --user astralbot

```

* [javascript npmjs](https://www.npmjs.com/package/astralbot)

```
npm install astralbot
```

You will find instructions to build your bot on the readme of the library.

Keep in mind you have to start an instance of the engine (with docker) then start your script.


The goal of the engine is to start a game and interact with it to configure it as describe in a json file (game script).  
Then the engine load an inference model to predict the current game player state (gaming, looser, winner) for each frames.  
A delay is set in the game script to select the loop frequency (in ms).  
At each loop the engine send a screenshot of the game and predict the player state.  
The remote bot send an action which will be executed immediatly.

The engine states can be:

| state         | description                                        | after         |
| ------------- | -------------------------------------------------- | ------------- |
| uninitialized | init() must be called                              | startup       |
| initialized   | ready to start a game                              | uninitialized |
| starting      | the selected game is starting                      | initialized   |
| started       | the game is started but not configured             | starting      |
| ready         | the game is started and configure, model is loaded | started       |
| stopped       | the game is stopped by win, loose or timer         | ready         |

The player states can be:

| state         | description                                        | after         |
| ------------- | -------------------------------------------------- | ------------- |
| uninitialized | the game is not started/ready                      | startup       |
| ready         | the player is able to do something                 | started       |
| looser        | the player loose the game and the engine stop it   | started       |
| winner        | the player win the game and the engine stop it     | started       |

The backend list:

| backend       | description                                        | status          |
| ------------- | -------------------------------------------------- | --------------- |
| browser       | headless web brower                                | supported       |
| process       | start and manage a game process (OS side)          | not implemented |
| emulator      | start and manage an emulator (android, gba)        | TBD             |

The backend methods list:

| method        | description               | status              |
| ------------- | ------------------------- | ------------------- |
| clickAt(x,y)  | click/touch on the screen | support for browser |

The goal list:

| goal      | description                                 | status              |
| --------- | ------------------------------------------- | ------------------- |
| speedrun  | the engine keep the time to finish the game | support for browser |
| score     | the engine track the score from the game    | not implemented     |

Build image:
------------

```
docker build --rm -t astralbot-engine .
```

Run container in production:
----------------------------

```
docker run -ti --rm -p 8080:8080 --name AstralBot astralbot-engine
```

Where to find the community?
============================

A discord server is availble, let me know to get the link.
