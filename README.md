# Neural Tetris

Neural Tetris is a pet project I created in order to familiarize myself with deep learning. The project includes:

 * A fully playable version of Tetris via the web browser
 * A python web server backend for storing user games
 * A rethinkdb instance with example data
 * A script for training a neural network to play like a specified user

# Implementation Details

## The Game

The game logic is programmed using JavaScript and utilizes an HTML5 canvas for rendering the game. While a user is playing the game, data will be sent in real-time to a web server using websockets where it will be pushed to a database for training a neural network.

## Gameplay details

### Controls
 * <kbd>&larr;</kbd> - Move tetrimino left
 * <kbd>&rarr;</kbd> - Move tetrimino right
 * <kbd>&darr;</kbd> - Soft drop
 * <kbd>X</kbd> - Rotate tetrimino clockwise
 * <kbd>Z</kbd> - Rotate tetrimino counterclockwise

### Scoring
 * Single line clear - 100 &#215; level
 * Double line clear - 300 &#215; level
 * Triple line clear - 500 &#215; level
 * Tetris line clear - 800 &#215; level
 * Combo clear - 50 &#215; count &#215; level
 * Soft drop - 1 point per unit dropped

### Advanced Players

Regarding the game itself, I referred to the [Tetris Wiki](http://tetris.wikia.com/wiki/Tetris_Wiki) in order to specify game behaviors, scoring, and color themes. I have chosen not to implement a hard drop or scoring for T-spins.

## The Server

TODO: Description of functionality

## The Brain

TODO: NNet architecture

# Setting up your own instance

## Installing Prerequisites

Install the following prerequisite libraries:

 * [Rethinkdb](https://www.rethinkdb.com/docs/install/)
 * [Python 3.4+](https://www.python.org)
   * The following python libraries:
     * [Numpy](http://www.numpy.org/)
     * [Pandas](http://pandas.pydata.org/)
     * [websockets](https://pypi.python.org/pypi/websockets)
     * [rethinkdb python driver](https://www.rethinkdb.com/docs/install-drivers/python/)
     * [Keras](https://keras.io/)
       * One of the following backends:
         * [Tensorflow](https://www.tensorflow.org/install/)
         * [Theano](http://deeplearning.net/software/theano/install.html#install)
         * [CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/Setup-CNTK-on-your-machine)

## Spinning up the Database

Navigate to the ```server``` directory and start the rethinkdb engine:

```bash
rethinkdb
```

Once the database server is running on your local machine, you will need to restore the test data by executing the following command in the same directory as above:

```bash
rethinkdb restore moves.tar.gz
```

The engine should pick up on the existing data and you can test that this is working by navigating a browser instance to: ```http://localhost:8080/```. You should see a web interface and under the "Tables" heading, it should read "1/1 tables ready."

You can verify that the import worked correctly by navigating to the "Data Explorer" tab on the top menu bar of the web interface and typing the following command:

```javascript
r.db('Tetris').table('moves').group('userId','gameId').pluck('userId','gameId').count()
```

And pressing the 'Run' button. The resulting "Table view" should look like the following:

![A listing of the example games in the test database](images/exampleData.png "Example Database")

## Starting the Web server

While still in the ```server``` directory initiate the web server by running the following command:

```bash
python TetrisServer.py
```

This script will listen for incoming websockets and respond appropriately. There are currently four modes:

 * Recording player's actions of a new game
 * Replaying player's action of a previously stored game
 * Debug replaying of the board state of a previously stored game
 * Playing a new game with a saved AI instance

The browser's first message to the webserver will dictate which path is executed by that instance of the web server. The good news is with the newer asyncio library of Python, we only need to execute this script once and it will allow for multiple incoming connections.

## Testing the game

At this point, you should be able to load one of the html files in the root directory of the project, their names should correspond to the actions above for the web server.

# Contributing

I have not gotten this far yet, but if you are interested shoot me an email or leave a comment somewhere on the project and we can figure something out.

# Author

Dan Maljovec
