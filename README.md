# Rules
Protocol based on generally accepted chess rules with minor clarifications and differences
- No build-in (on proto level) alliances, free for all
- En passant disallowed?

# Proto TODO
- [ ] Out of game session
  - [ ] matchmaking queque
    - [x] Register player
    - [ ] Leave player
    - [x] Inform queue members
      - [x] New player join
      - [x] Player leave
      - [x] All 4 players are found, game start
        - [x] Check alive players before start game session, drop dead
- [ ] Active game session
  - [ ] Basic move
  - [ ] Special move
    - [ ] Promotion
    - [ ] En Passant
    - [ ] Castling
  - [ ] Player timer
  - [ ] Reconnect on connection lost
  - [ ] Drawing like this  
  ![](BLOB/drawing.png)
 
