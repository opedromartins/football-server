# Running the Football Server

## Cloning the Repository

Clone this repository.

```bash	
git clone https://github.com/opedromartins/football-server.git
```

Navigate to the docker folder inside the repository.

```bash
cd football-server
```

## Building the Image

Build the Docker image with using the following command:

```bash
docker build -t football-server .
```

## Running the Container

To run the Docker container, utilize the provided run script with the following parameters:

```bash
./run.sh football-server [--rm] [--nvidia]
```

- `<image-name>`: The name you assigned to the Docker image during the build process.
- `--rm`: Automatically remove the container when it exits.
- `--nvidia`: Run the container with NVIDIA GPU support.

The `checkpoints` folder will be linked to the `/root/checkpoints` folder within the container. This linking is a simplified way for you to use your own trained models.

Put your trained models in the `checkpoints` folder and they will be available to the container.

## Join the container

This is a step-by-step guide on how to join a running container using Docker. You will use it later.

#### List Running Containers
Open your terminal or command prompt and list the running containers using the following command:
```bash
docker ps
```
This will display a list of all running containers along with their IDs, names, and other details.

#### Find Container ID or Name
Identify the container you want to join and note its Container ID or Name.

#### Join the Container
To join the running container, use the following command:
```bash
docker exec -it <container_id_or_name> /bin/bash
```
Replace `<container_id_or_name>` with the actual ID or name of your container.

- The `-it` flag stands for interactive mode, allowing you to interact with the container.
- `/bin/bash` specifies the shell you want to use.

## Running the server

To run the server, use the following command:

`python -m gfootball.play_game --players "remote:left_players=1,port=6000;bot:right_players=1" --action_set=full`

This command is used to initiate a football game, simulating a match between two players. One player is controlled remotely (left), and the other is controlled by a bot (right).

##### Parameters:

- `python -m gfootball.play_game`: This part of the command runs the `play_game` module from the `gfootball` package.

- `--players "remote:left_players=1,port=6000;bot:right_players=1"`: This parameter defines the players in the game. It consists of two players:
  - `remote`: Represents the player controlled remotely.
    - `left_players=1`: Specifies that there is one player on the left side (controlled remotely).
    - `port=6000`: Sets the communication port to 6000 for the remote player.
  - `bot`: Represents the player controlled by a bot.
    - `right_players=1`: Specifies that there is one player on the right side controlled by a bot.

- `--action_set=full`: This parameter sets the action set to "full," indicating that the players have access to the complete set of possible actions in the game.

## Running the agent

Open another terminal and join the running container

Run the entrypoint.py file using the following command:

`python entrypoint.py`

This command executes the player script, connecting to the game server and interacting with the environment.

If everything is set up correctly, a window will open displaying the game. You should see the agent playing against the bot.

### Modifying the agent

Depending on how you trained your agent, you will have to modify the parameters inside the `entrypoint.py` file.

Open the `entrypoint.py` file and adjust the player configuration as needed. Some key configurations include:
   - `checkpoint`: Your checkpoint path.
   - `action_set`: Your agent action set, may be `default`, `full` or another one.
   - `policy`: The policy you trained your agent on.
   - `players`: Set the number of players your agent controls.
   - `player_config`: Configure your agent's settings, such as index, action set, representation, etc.
   - `agent`: Load your trained agent with the appropriate settings.


## Some comments

- That repository was created for running the `baseline_ppo` agent, for any other agent you will have to adapt it.