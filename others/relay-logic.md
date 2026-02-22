# BPF and Relay control
This is the documentation of the relay and RF switches state coordination.

## Truth table (NOR)
| RADIO_TX | FORCE_BYPASS | Logic Result | Behavior |
:---|---|---|---:
| 0 | 0 | 1 | VIA BPF |
| 0 | 1 | 0 | BYPASS |
| 1 | 0 | 0 | BYPASS |
| 1 | 1 | 0 | BYPASS |

## RX/Via BPF states:
| Circuit | Connected to  | Power |
:---------|---------------|--------:
| Relays  | NO | + |
| Isolation | NO | + |

## TX/Bypass states:
| Circuit | Connected to  | Power |
:---------|---------------|--------:
| Relays  | NC | - |
| Isolation | NC | - |

# LNA Control
## Truth Table (AND)
| BPF_ON | LNA_ON | Logic Result | Behavior |
:---|---|---|---:
| 0 | 0 | 0 | OFF |
| 0 | 1 | 0 | OFF |
| 1 | 0 | 0 | OFF |
| 1 | 1 | 1 | ON |

## LNA On
| Circuit | Connected to  | Power |
:---------|---------------|--------:
| V.REG.  | EN | + |
| LNA SW  | NO | + |

## LNA Off
| Circuit | Connected to  | Power |
:---------|---------------|--------:
| V.REG.  | EN | - |
| LNA SW  | NC | - |

The VHF/UHF selector is NORMALLY CONNECTED (NC) to the UHF BPF. By providing a 3V input to the VHF header, the BPF RF switches transition to NO (normally open) state and diverts the signal to the VHF chain.
