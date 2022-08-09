# talemaker-data
This repository holds the data from TaleMaker's database of stories

**talemaker_clients**
```
client_id VARCHAR (255) PRIMARY KEY,
name VARCHAR (30) NOT NULL,
time BIGINT NOT NULL,
avatar VARCHAR (50),
color VARCHAR (7)
```

**talemaker_games**
```
game_id VARCHAR (255) PRIMARY KEY,
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),

room_name VARCHAR(20) NOT NULL,
private BOOLEAN NOT NULL,
version VARCHAR(10) NOT NULL,
voting BOOLEAN NOT NULL,

stats_num_players INT NOT NULL,
stats_num_tokens INT NOT NULL,
stats_num_tokens_drawn_pp INT NOT NULL,
stats_num_tokens_drawn_discard INT NOT NULL,
stats_num_tokens_drawn INT NOT NULL,
stats_num_tokens_discard INT NOT NULL,
stats_num_turns INT NOT NULL, 
stats_num_characters INT NOT NULL,
stats_num_locations INT NOT NULL, 
stats_num_icons INT NOT NULL,
stats_num_plotpoints INT NOT NULL,

min_players INT NOT NULL,
max_players INT NOT NULL,
max_piles INT NOT NULL,
max_pp_slots INT NOT NULL,
num_samples INT NOT NULL,
min_tokens INT NOT NULL, 
num_locations INT NOT NULL,
num_characters INT NOT NULL,
num_revotes INT NOT NULL,
min_plotpoints INT NOT NULL,

time_piles INT NOT NULL,
time_drawn INT NOT NULL,
time_plotpoint INT NOT NULL,
time_vote INT NOT NULL,
time_vote_trans INT NOT NULL,
time_decay_factor INT NOT NULL,

score_fusion VARCHAR(4) NOT NULL,
min_verbs INT NOT NULL,
min_others INT NOT NULL,
base_gold INT NOT NULL,
base_diamonds INT NOT NULL,
winner_factor INT NOT NULL,
top_related INT NOT NULL,
bags_labels text[],
bags_distribs INT[],
bags_random INT[][],
history_window INT NOT NULL,

discard_relevance INT NOT NULL,
drawn_relevance INT NOT NULL,
pp_relevance INT NOT NULL,

senti_window INT NOT NULL,
senti_labels text[] NOT NULL,
senti_enabled BOOLEAN NOT NULL,

time_start BIGINT NOT NULL,
time_end BIGINT NOT NULL,
time BIGINT NOT NULL
```

**talemaker_players**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
gold INT NOT NULL,
diamonds INT NOT NULL,
UNIQUE (game_id, client_id)
```

**talemaker_states**
```
state_id BIGSERIAL PRIMARY KEY,
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
state INT NOT NULL,
turn INT NOT NULL,
time BIGINT NOT NULL
```

**talemaker_rewards**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),

gold INT NOT NULL,
diamonds INT NOT NULL,
turn INT NOT NULL
```


**talemaker_turn_times**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
turn INT NOT NULL,
time_turn BIGINT NOT NULL,
time_piles BIGINT NOT NULL,
time_drawn BIGINT NOT NULL,
time_plotpoint BIGINT NOT NULL
```

**talemaker_tokens**
```
token_id BIGSERIAL PRIMARY KEY,
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),

pid INT NOT NULL,

wnid VARCHAR (9), 
name VARCHAR (30),
lemma VARCHAR (100),
pile VARCHAR (20),
bag  VARCHAR (10),
icon VARCHAR (50),

turn INT,
pp_turn INT,
drawn_turn INT,
discard_turn INT
```

**talemaker_hands**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
token_id BIGINT REFERENCES talemaker_tokens(token_id),
turn INT NOT NULL
```

**talemaker_stories**
```
story_id BIGSERIAL PRIMARY KEY,
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),

name VARCHAR (255) NOT NULL,
num_plotpoints INT NOT NULL,
num_tokens INT NOT NULL,
num_synsets INT NOT NULL,
num_unique_synsets INT NOT NULL,
num_tokens_drawn INT NOT NULL, 
num_evaluations INT NOT NULL
```

**talemaker_story_names**
```
story_id BIGINT REFERENCES talemaker_stories(story_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),

name VARCHAR (100) NOT NULL,
votes INT NOT NULL
```

**talemaker_plotpoints**
```
plotpoint_id BIGSERIAL PRIMARY KEY,
story_id BIGINT REFERENCES talemaker_stories(story_id),
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
location_id BIGINT REFERENCES talemaker_tokens(token_id),

pid INT NOT NULL, 

turn INT NOT NULL,
winner boolean,
votes INT NOT NULL,
num_slots INT NOT NULL,
text TEXT,
index INT,

name VARCHAR (30),
avatar VARCHAR (50),
color VARCHAR (7)
```

**talemaker_slots**
```
slot_id BIGSERIAL PRIMARY KEY,
plotpoint_id BIGINT REFERENCES talemaker_plotpoints(plotpoint_id),
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
token_id BIGINT REFERENCES talemaker_tokens(token_id),

slot_type VARCHAR (30), 
view BOOLEAN, 
index INT
```

**talemaker_plotpoints_voters**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
plotpoint_id BIGINT REFERENCES talemaker_plotpoints(plotpoint_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id)
```

**talemaker_piles**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
category VARCHAR (20),
turn INT NOT NULL
```

**talemaker_voter_trans**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
vote INT NOT NULL,
turn INT NOT NULL
```

**talemaker_voter_change**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
vote INT NOT NULL,
turn INT NOT NULL
```

**talemaker_voter_finish**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
vote INT NOT NULL,
turn INT NOT NULL
```

**talemaker_evaluation**
```
story_id BIGINT REFERENCES talemaker_stories(story_id),
client_id VARCHAR (255) REFERENCES talemaker_clients(client_id),
question VARCHAR (10) NOT NULL,
answer INT NOT NULL
```

**talemaker_sentiments**
```
game_id VARCHAR (255) REFERENCES talemaker_games(game_id),
senti VARCHAR(3) NOT NULL,
mean REAL NOT NULL,
std REAL NOT NULL,
turn INT NOT NULL
```
