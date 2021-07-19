# AstralBot

The AstralBot engine is an application to interact and stream a game for an external bot

At the startup a websocket is opened on port 8080 to let a remote bot start a game and interact with it to play.  
The engine send each video frame to the remote bot and your bot should send actions to do.


The first thing to do is to start an astralbot engine with docker.

```
docker run -ti --rm -p 8080:8080 --name AstralBot astralbot/astralbot-engine
```


How To made the remote bot?
===========================

We provide a library to interact with an engine for javascript and python.  
These library comes with examples on their respective github repositories.

* [python github](https://github.com/AstralBotAI/AstralBot-py)

```
pip install --user astralbot

```

* [javascript github](https://github.com/AstralBotAI/AstralBot-js)

```
npm install astralbot
```

Keep in mind you have to start an instance of the engine (with docker) then start your script.

The goal of the engine is to start a game and interact with it to configure it as describe in a json file (game script).  
Then the engine load an inference model to predict the current game player state (gaming, looser, winner) for each frames.  
A delay is set in the game script to select the loop frequency (in ms).  
At each loop the engine send a screenshot of the game and predict the player state and send it to clients.  
The remote bot send an action which will be executed immediatly.

The engine states can be:

| state         | description/meaning                                |
| ------------- | -------------------------------------------------- |
| uninitialized | init() must be called                              |
| initialized   | ready to start a game                              |
| starting      | the selected game is starting                      |
| started       | the game is started but not configured             |
| ready         | the game is started and configure, model is loaded |
| stopped       | the game is stopped by win, loose or timer         |

The player states can be:

| state         | description/meaning                                |
| ------------- | -------------------------------------------------- |
| uninitialized | the game is not started/ready                      |
| ready         | the player is able to do something                 |
| looser        | the player loose the game and the engine stop it   |
| winner        | the player win the game and the engine stop it     |

The backend list:

| backend       | description                                        | dev status      |
| ------------- | -------------------------------------------------- | --------------- |
| browser       | headless web brower                                | supported       |
| process       | start and manage a game process (OS side)          | not implemented |
| emulator      | start and manage an emulator (android, gba)        | TBD             |

The backend methods list:

| method        | description               | dev status          |
| ------------- | ------------------------- | ------------------- |
| clickAt(x,y)  | click/touch on the screen | support for browser |

The goal list:

| goal      | description                                 | status              |
| --------- | ------------------------------------------- | ------------------- |
| speedrun  | the engine keep the time to finish the game | support for browser |
| score     | the engine track the score from the game    | not implemented     |

Where to find the community?
============================

A discord is availble [here](https://discord.gg/Xq33rrHFue).
