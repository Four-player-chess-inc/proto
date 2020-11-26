# Rules
Protocol based on generally accepted chess rules with minor clarifications and differences
- No build-in (on proto level) alliances, free for all
- All opponent kings captured, last captured or сheckmated.

# Proto TODO
- [x] Beyond game session
  - [x] Matchmaking queque
    - [x] Register player
    - [x] Leave player
    - [x] Inform queue members on
      - [x] New player join
      - [x] Player leave
      - [x] All 4 players are found, game start
        - [x] Check alive players before start game session, kick deads
- [ ] Game session
  - [ ] Reconnect on connection lost
    - [ ] Pause/Unpause
  - [ ] Basic move
  - [ ] Special move
    - [ ] Promotion
    - [ ] En Passant
    - [ ] Castling
  - [ ] Player leave aka defeat
  - [ ] Player timer
  - [ ] Drawing like this  
  ![](BLOB/drawing.png)
 
