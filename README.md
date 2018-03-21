DisTris
=======

A multiplayer Tetris like game on Android.

![Logo][logo]

This has been a group project in Distributed Systems in 2016 by Mischa Brander, Alexander Canals, Aaron Keller, Sebastian Keller, Sandro Meier, Pirmin Schmid.


# Game description
First released in 1984, [Tetris][tetris] has been a huge success. Many versions for over 60 different computer platforms have been released over the decades. Many multiplayer versions let players compete by points but let them play in their own space.

Our game variant splits the space for each user into three parts. As a player, the center is only visible to myself, left and right shared spaces are visible to and played with my neighbors on the left and right.

This offers a very enjoyable twist to the game but also some technical challenges as mentioned below.

![Screenshot][screenshot]


# Short project description
A modular design of the components allowed us working on various parts of the software in parallel. Additionally, various modules may be usable in other applications.

## Lobby
A classic game lobby is offered. A player starts broadcasting an invitation to a new game (using UDP). Players in the same subnet can join the game. Upon start of the actual games, each player is connected with two "neighbors" to exchange relevant events in the shared spaces (TCP). Thanks to these P2P connections without need for a central server, the game easily scales to many users in the same subnet. Scores are broadcasted with UDP.

## GameEngine
Maintaining a consistent state in the shared spaces of the board is of importance. We used dynamic vector clocks to order incoming events from user input and neighbors (via network). In contrast to single player versions, up to two Tetrominos can be falling in the shared space which needed proper collision detection/prevention. Changes in the shared state trigger again messages to the neighbors. This includes also proper removal of rows in the shared space of neighbors (our "row clearance exchange protocol").

## GfxEngine
Graphics was periodically updated based on the defined state in the game engine.

## Network
Low-level modules for UDP and TCP connection and messaging.

## Sound
We were lucky to have a composer in our group (Sandro Meier) to create a unique sound track for our game.

## AppBroadcaster
The various modules of the application communicate with Android intents. Serializable data containers were defined where needed to allow smooth event/data transfer within the app and also over the network. Specific filters allowed triggering of events upon reception of defined messages.

# Testing
We quickly learned that testing of a distributed application comes with its own challenges. Writing a game, however, comes with the advantage that some of the testing can be done by just playing the game in a group. :-)


# Media
- short [video clip][clip1] with sound (MP4)
- longer [video clip][clip2] during testing without sound (MP4)

The game is not available in the Android store. The source code may become available sometime in the future.

[Feedback][feedback] welcome.

[logo]:bin/logo.png
[tetris]:https://en.wikipedia.org/wiki/Tetris
[screenshot]:bin/screenshot.png
[clip1]:bin/clip1.mp4
[clip2]:bin/clip2.mp4
[feedback]:mailto:mailbox@pirmin-schmid.ch?subject=DisTris
