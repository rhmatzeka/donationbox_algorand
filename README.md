# Donation Box: Algorand Smart Contract

A learning project for writing **Algorand** smart contracts in Python with **AlgoPy** and **AlgoKit**. The goal is a simple, transparent "donation box": anyone can donate, and anyone can check how much has been collected.

> **Status:** the contract committed in `contract.py` is still the AlgoKit starter, with a single `hello(name)` method. The donation box below is the planned design and hasn't been committed yet.

## Planned design

The contract keeps one number in its global state, `total_donations` (in microAlgos), and exposes three ARC-4 methods:

| Method | What it does |
| --- | --- |
| `donate(amount) -> string` | Adds `amount` to the total and returns a thank-you message |
| `get_total() -> uint64` | Returns the total collected so far (read-only) |
| `reset() -> string` | Sets the total back to 0. In a real app, only the contract creator should be allowed to call this. |

```python
from algopy import ARC4Contract, UInt64, String
from algopy.arc4 import abimethod


class DonationBox(ARC4Contract):
    def __init__(self) -> None:
        # Runs once, when the contract is created
        self.total_donations = UInt64(0)

    @abimethod()
    def donate(self, amount: UInt64) -> String:
        self.total_donations += amount
        return String("Thank you for your donation!")

    @abimethod()
    def get_total(self) -> UInt64:
        return self.total_donations

    @abimethod()
    def reset(self) -> String:
        self.total_donations = UInt64(0)
        return String("Donation box has been reset!")
```

This small contract shows the three basics of on-chain state: **creating** it (`__init__`), **changing** it (`donate`, `reset`), and **reading** it (`get_total`).

## Getting started

You need Python 3.12+, Docker, [AlgoKit](https://github.com/algorandfoundation/algokit-cli) 2+, and Poetry.

```bash
cd projects/donation_box
algokit project bootstrap all                              # install dependencies
algokit generate env-file -a target_network localnet       # create .env.localnet
algokit localnet start                                     # start a local Algorand network
algokit project run build                                  # compile the contract
algokit project deploy localnet                            # deploy it
```

## Project structure

| Path | What it is |
| --- | --- |
| `projects/donation_box/smart_contracts/donation_box/contract.py` | The smart contract |
| `projects/donation_box/smart_contracts/donation_box/deploy_config.py` | Deploys the contract and calls it once |
| `projects/donation_box/README.md` | AlgoKit's full getting-started guide |

## License

Released under the [MIT License](LICENSE).
