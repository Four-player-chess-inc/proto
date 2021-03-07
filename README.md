# Rules
Protocol based on generally accepted chess rules with minor clarifications and differences
- No build-in (on proto level) alliances, free for all
- Pawn promoted on 8 line
- En Passant forbidden (see previous line)
- Stalemated, checkmated or king captured player loses
- Stalemate and checkmate states can be interrupt until the stalemated or checkmated player's turn comes

Game session
If disconnect, wait timer
If leave, lose


# Proto TODO
- [x] Beyond game session
  - [x] Handshake
  - [x] Matchmaking queque
    - [x] Register player
    - [x] Leave player
    - [x] Inform queue members on
      - [x] New player join
      - [x] Player leave
      - [x] All 4 players are found, game start
        - [x] Check alive players before start game session, kick deads
- [ ] Game session
  - [x] Reconnect on connection lost
    - [x] Pause/Unpause
  - [ ] Basic move
  - [ ] Special move
    - [ ] Promotion
    - [ ] Castling
  - [ ] Player leave aka defeat
  - [ ] Player timer
  - [ ] Drawing like this  
  ![](BLOB/drawing.png)
 
