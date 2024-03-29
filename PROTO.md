# Four player chess protocol
Connection design protocol. Only one player per connection

## Matchmaking

Register players in matchmaking queue  
Request
```json
{
    "matchmaking_queue": {
        "register": {
            "name": "h04x"
        }
    }
}
```
a
Response variants  
Session_id for reconnect to active game
```json
{
    "matchmaking_queue": {
        "register": {
            "ok": {} 
        }
    }
}
```
```json
{
    "matchmaking_queue": {
        "register": {
            "error": {
                "bad_name": {
                    "description": "error desc. e.g. too long"
                }
            }
        }
    }    
}
```
```json
{
    "matchmaking_queue": {
        "register": {
            "error": {
                "already_registered": {
                    "description": "already in mm queue or in game"
                }
            }
        }
    }    
}
```
```json
{
    "matchmaking_queue": {
        "register": {
            "error": {
                "unspecified_error": {
                    "description": "error desc. e.g. internal error"
                }
            }
        }
    }          
}
```

Leave player from queue
Request
```json
{
    "matchmaking_queue": {
        "leave": {}
    }
}
```
No response provided

When four players were registered, server broadcast heartbeat check
```json
{
    "matchmaking_queue": {
        "heartbeat_check": {
            "timeout": 5
        }
    }    
}
```

Each payer must response
```json
{
    "matchmaking_queue": {
        "heartbeat_check": {}
    }    
}
```

If player does not confirm heartbeat he will be kicked from queue
```json
{
    "matchmaking_queue": {
        "player_kick": {
            "description" : "e.g. due to heartbeat timeout"
        }
    }
}
```
Each player who confirmed heartbeat but timeout, return to matchmaking
```json
{
    "matchmaking_queue": {
        "continue": {}
    }
}
```


## Game session

Starting game session  
This message broadcast to all players, reconnect_id is unique for every player 
Also this message send to reconnected player if countdown not over.
For spectators reconnect_id empty
```json
{
    "game_session": {
        "init": {
            "countdown": 10,
            "reconnect_id": "qweASDersscv",
            "start_positions": {
                "Red": {
                    "player_name": "h04x",
                    "left_rook": "d1",
                },
                "Blue": {
                    "player_name": "Queen",
                    "left_rook": "a11"
                },
                "Yellow": {
                    "player_name": "tux",
                    "left_rook": "k14"
                },              
                "Green": {
                    "player_name": "Bobby",
                    "left_rook": "n4"
                }          
            }
        }
    }
}
```

Okay, after ```"countdown"``` seconds, server call the first player to make a move. Broadcasted
```json
{
    "game_session": {
        "update" : {
            "move_call": {
                "call" : {
                    "color": "Red",
                    "time": 50,
                    "timer_2": 0
                }
            },
            "move_previous": {
                "no_move": {}
            },
            "players_states": {
                "Red": {
                    "no_state": {}
                },
                "Blue": {
                    "no_state": {}
                },
                "Yellow": {
                    "no_state": {}
                },
                "Green": {
                    "no_move": {}
                },                                                
            }
        }
    }
}
```

Player leave
```json
{
    "game_session": {
        "player_leave": {}
    }
}
```

Player reconnect 
```json
{
    "game_session": {
        "reconnect": {
            "reconnect_id": 123,
        }
    }
}
```
response
```json
{
    "game_session": {
        "reconnect": {
            "ok": {},
        }
    }
}
```
```json
{
    "game_session": {
        "reconnect": {
            "error" : {
                "type": "game over",
                "description": ""
            }
        }
    }
}
```
```json
{
    "game_session": {
        "reconnect": {
            "error" : {
                "type": "unspecified_error",
                "description": "error desc. e.g. internal error"
            }
        }
    }
}
```


Move variants
basic move
```json
{
    "game_session": {
        "move": { 
            "basic": {
                "from": "e2",
                "to": "e4"
            }
        }
    }
}
```

capture
```json
{
    "game_session": {
        "move": {
            "capture": {
                "from": "e2",
                "to": "d3",
            }   
        }
    }
}
```

promotion
```json
{
    "game_session": {
        "move": {
            "promotion": {
                "from": "e7",
                "to": "e8",
                "into": "Queen"
            }        
        }
    }
}
```

castling
```json
{
    "game_session": {
        "move": {
            "castling": {
                "rook": "d1",
            }                  
        }
    }
}
```
Response

On error occurred, sender recv error variants
```json
{
    "game_session": {
        "move": {
            "error" : {
                "forbidden_move": {
                    "description": "e.g. another piece on your way"
                }
            }
        }
    }
}
```
```json
{
    "game_session": {
        "move": {
            "make": {
                "error" : {
                    "unspecified_error": {
                        "description": "error desc. e.g. internal error"
                    }
                }
            }
        }
    }
}
```

Update variants, when first move call
```json
{
    "game_session": {
        "update" : {
            "move_call": {
                "call": {
                    "color": "Green",
                    "time": 50,
                    "timer_2": 0
                }
            },
            "move_previous": {
                "no_move": {}
            },
            "players_states": {
                "Red": {
                    "no_state": {}
                },
                "Blue": {
                    "no_state": {}
                },
                "Yellow": {
                    "no_state": {}
                },
                "Green": {
                    "no_state": {}
                },                                                
            }
        }
    }
}
```

```json
{
    "game_session": {
        "update" : {
            "move_call": {
                "call": {
                    "color": "Green",
                    "time": 50,
                    "timer_2": 0
                }
            },
            "move_previous": {
                "basic": {
                    "from": "e2",
                    "to": "e4"
                }
            },
            "players_states": {
                "Red": {
                    "lost": {
                        "remaining_pieces": "clear" | "turn_to_stone"
                    }
                },
                "Blue": {
                    "check": {}
                },
                "Yellow": {
                    "checkmate": {}
                },
                "Green": {
                    "stalemate": {}
                }
            }
        }
    }
}
```

when game is over update contain no_call
```json
{
    "game_session": {
        "update" : {
            "move_call": {
                "no_call": {}
            },
            "move_previous": {
                "basic": {
                    "from": "e2",
                    "to": "e4"
                }
            },
            "players_states": {
                "Red": {
                    "lost": {
                        "remaining_pieces": "clear"
                    }
                },
                "Blue": {
                    "lost": {
                        "remaining_pieces": "clear"
                    }
                },
                "Yellow": {
                    "lost": {
                        "remaining_pieces": "clear"
                    }
                },
                "Green": {
                    "no_state": {}
                }
            }
        }
    }
}
```