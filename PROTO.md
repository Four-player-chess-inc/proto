# Four player chess protocol
Connection design protocol. Only one player per connection

## Handshake
Get info Request
```json
{
    "handshake": {
        "get_info": {
            "request": {}
        }
    }
}
```

Get info responses
```json
{
    "handshake": {
        "get_info": {
            "ok": {
                "protocol": {
                    "supported_version": ["0","1"]
                }
            }
        }
    }
}
```
```json
{
    "handshake": {
        "get_info": {
            "error": {
                "unspecified_error": {
                    "description": "error desc. e.g. internal error"
                }       
            }
        }
    }
}
```

Connect Request
```json
{
    "handshake": {
        "connect" : {
            "client" : {
                "name": "fpc-client-web",
                "version": "0.0.1",
                "protocol": {
                    "version": "0"
                }
            }
        }
    }
}
```

Connection possible responses
```json
{
    "handshake": {
        "connect" : {
            "ok": {
                "server" : {
                    "name": "fpc-server-rs",
                    "version": "0.0.1"
                }                
            }
        }
    }
}
```
```json
{
    "handshake": {
        "connect" : {
            "error": {
                "unsupported_protocol_version" : {
                    "description": ""
                }                
            }
        }
    }
}
```
```json
{
    "handshake": {
        "connect" : {
            "error": {
                "unspecified_error": {
                    "description": "error desc. e.g. internal error"
                }               
            }
        }
    }
}
```
## Matchmaking

Register players in matchmaking queue  
Request
```json
{
    "matchmaking_queue": {
        "player_register": {
            "name": "h04x"
        }
    }
}
```

Response variants  
Session_id for reconnect to active game
```json
{
    "matchmaking_queue": {
        "player_register": {
            "ok": {} 
        }
    }
}
```
```json
{
    "matchmaking_queue": {
        "player_register": {
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
        "player_register": {
            "error": {
                "already_registered": {
                    "description": "player already in mm queue or in game"
                }
            }
        }
    }    
}
```
```json
{
    "matchmaking_queue": {
        "player_register": {
            "error": {
                "handshake": {
                    "description": "pass handshake first"
                }
            }
        }
    }          
}
```
```json
{
    "matchmaking_queue": {
        "player_register": {
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
        "player_leave": {}
    }
}
```
No response provided

When four players were registered, server broadcast heartbeat check
```json
{
    "matchmaking_queue": {
        "heartbeat_check": {}
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
If all players ready, server start game session, otherwise dead players dropped and server is waiting for a replacement in their place.  
If player dropped while his connection still established, server send kick message
```json
{
    "matchmaking_queue": {
        "player_kick": {
            "discription" : "e.g. due to heardtbeat timeout"
        }
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
                "color": "Red",
                "time": 50,
                "timer_2": 0
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

On error occured, sender recv error variants
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
                "color": "Green",
                "time": 50,
                "timer_2": 0
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
                "color": "Green",
                "time": 50,
                "timer_2": 0
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
                    "chech": {
                        "from": ["e3", "a8"]
                    }
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
