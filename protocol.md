- actor creates a poker game

  *Client*: `NEW_GAME [<context-of-hand>, ...]`

- actor joins a poker game

  *Client*: `JOIN_AS <nick>`

  *Server*: `JOINED <nick>`

- actor leaves a poker game

  *Client*: `PART`

  *Server*: `PARTED <nick>`

- actor plays a card on the hand

  *Client*: `PLAY <card>`

- actor (or system) informs players about the context of the hand

  *Client*: `NEW_HAND <context>`

  *Server*: `NEXT_HAND <context>`

- actor (or system) informs players about the end-of-game

  *Client*: `END_GAME`

  *Server*: `GAME_ENDED`

- actor is informed when any player joins the game

  *Server*: `JOINED <nick>`

- actor is informed when any player updates their identity (e.g. name)

  *Server*: `PLAYER_UPDATED_NICK <old-nick> <new-nick>`

- actor is informed when any player leaves the game

  *Server*: `PARTED <nick>`

- actor is informed when a new hand is in play

  *Server*: `NEXT_HAND <context>`

- actor is informed when the hand is exposed (cards turned over)

  *Server*: `CARDS {<nick>: <card>, ...}`

- actor is informed when the hand is finished (fixed/persisted, no further plays on the hand)

  *Server*: `HAND_ENDED`

- actor is informed when the hand needs replayed

  *Server*: `REPLAY`

- actor is informed when end-of-game criteria has been met (or invoked by an actor)

  *Server*: `GAME_ENDED`

- system informs players about when a new hand is in play

  *Server*: `NEXT_HAND <context>`

- system informs players about when a hand is exposed (and therefore, what the other players played)

  *Server*: `CARDS <nick>:<card>, ...`

- system informs players when another player has played a card

  *Server*: `PLAYED <nick>`


Example
-------

k = karen's client

m = michael's client

s = server

- k -> s `JOIN_AS karen`
- m -> s `JOIN_As michael`
- k <- s `JOINED michael`
- m <- s `JOINED karen` (?)
- m -> s `NEW_GAME`
- m -> s `NEW_HAND "Set up server"`
- k <- s `NEXT_HAND "Set up server"`
- k -> s `PLAY 3`
- m <- s `PLAYED karen`
- m -> s `PLAY 2`
- k <- s `PLAYED michael` (?)
- k <- s `CARDS {"karen": 3, "michael": 2}`
- m <- s `CARDS {"karen": 3, "michael": 2}`
- k <- s `REPLAY`
- m <- s `REPLAY`
- k -> s `PLAY 2`
- m <- s `PLAYED karen`
- m -> s `PLAY 2`
- k <- s `PLAYED michael` (?)
- k <- s `CARDS {"karen": 2, "michael": 2}`
- m <- s `CARDS {"karen": 2, "michael": 2}`
- k <- s `HAND_ENDED`
- m <- s `HAND_ENDED`
- m -> s `END_GAME`
- k <- s `GAME_ENDED`
- k -> s `PART`
- m <- s `PARTED karen`
