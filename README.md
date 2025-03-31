# Chews Protocol


![chews-logo-600](https://github.com/user-attachments/assets/17d00515-e989-4583-af2d-a4989279ca30)


## Overview

Chews Protocol is a modular Token Curated Registry (TCR) voting protocol that allows developers to create custom voting systems by composing different module types. Unlike traditional monolithic TCR systems that follow a one-size-fits-all approach, Chews ("choose") enables flexible and use-case-driven governance through its modular architecture.

The protocol can be thought of as an "ACR" (Anything Curated Registry) system, as it allows curation of registries based on any criteria, not just tokens. This flexibility makes it suitable for a wide range of decision-making and governance applications.

## Development Stage

**⚠️ EXPERIMENTAL: Version 0 ⚠️**

Chews Protocol is currently in an early, experimental stage (V0). While it is functional and already in use by several projects, users should be aware that the architecture, interfaces, and implementation details may change significantly in future versions.

## Key Features

- **Fully Modular** - Create custom voting strategies based on combinations of four module types.
- **Flexible Design** - Functions as an "ACR" (Anything Curated Registry) system that can curate registries based on any criteria.
- **Composable Systems** - All voting systems (Contests) share a standard interface, making them composable with each other.
- **Complex Voting Patterns** - Orchestrate serial or parallel voting patterns for sophisticated governance needs.
- **Developer-Friendly** - Reduces cognitive overhead by separating concerns into distinct module types.
- **Procedural or Continuous** - Support for both time-bound procedural voting processes and ongoing continuous voting systems.
- **Chain Agnostic** - Deployable on any EVM-compatible blockchain.

## Current Implementations

The following projects are already using Chews Protocol:

1. **Grant Ships** - A competitive ecosystem funding platform that uses Chews to vote or rate grant programs following allocation rounds. Grant Ships utilized 3 separate voting systems:
    * Standard TCR vote for ARB that allowed Game Facilitators with a the Facilitator Hat Protocol NFT. 
    * SBT TCR for community members who were assigned voting scores for participation in the game.
    * Dual token voting. Allowed for ARB and the Grant Ships game SBT to be utilized in a parallel vote. 

2. **Gitcoin** - A custom version of GrantShips that implements a rubric voting system where judges rate grants programs using specific criteria.
    * Rubric Votes. Utilizes an innovative Max-Votes-Per-Choice model, and allows judges to vote on each choice using a percentage instead of a token weighted value. This allowed judges to rate each grant program on their own merits instead of relative to each other. 
    * AI-Assisted Public Vote. We generated a Merkle tree from GTC balances on mainnet and allowed users to vote using that balance on an L2 (Arbitrum).
    * Judge selection vote. This is a standard election that once again uses GTC balances on mainnet to elect judges. Once the vote is completed, the contest automatically mints hats and assigns them to the winners.  

3. **ask.haus** - Collaboration with DAOhaus. Chews was utilized in a proof of concept application for testing various UX patterns around fast and convenient TCR voting within Moloch DAOs. We implemented 2 patterns:
    * Poll. With pre-populated choices. The idea is to be able to create a fast poll for your DAO in under 2 minutes, and have voters vote on it in less than two minutes.  
    * Contest. Not to be confused with the poorly named Chews Protocol Contest.sol. This is a dual round system where Moloch DAO members can create choices, and then members can vote on those choices. Like the Poll, this system was designed to executed as quickly and conveniently as possible. 

## Architecture

Chews Protocol is built around a modular architecture that centers on a high-level contract (Contest.sol) that composes four different types of modules:

### Contest Contract

The Contest contract bundles all the modules into a standard interface. It manages the state for:
- Voting stage (if procedural)
- Continuous vs. procedural status
- Vote retraction rules
- Other high-level rules for the voting system

### Module Types

#### 1. Choices Module
Manages the items that users will be voting on. It handles:
- Choice creation and management
- Who can create choices
- Registry of voting options

#### 2. Points Module
Determines who can vote and how much voting power they have:
- Can reference token balances
- Can use Merkle proofs
- Supports any custom voting power determination logic

#### 3. Votes Module
Controls how voting happens and its immediate effects:
- Voting mechanisms (burning, staking, regular voting)
- Side effects of voting
- Vote tabulation rules

#### 4. Execution Module
Defines what happens as a result of the vote:
- Can mint roles onchain
- Can distribute funds
- Controls when and how execution happens

### Procedural vs. Continuous Contests

The Contest contract maintains a `ContestStatus` enum variable which can be:

**Procedural Flow**: 
1. Populating (for choices creation)
2. Voting (for submitting votes)
3. Finalized (ready to be executed)
4. Executed (after execution)

Procedural contests follow a set path through these stages, typically starting at Populating.

**Continuous Flow**:
If set as continuous from the start, the contest steps out of the procedural flow and can run indefinitely, allowing choices to be created and voted on simultaneously.

## Smart Contracts

### Core Contracts

- **Contest.sol** - The high-level contract that bundles modules and manages voting flow
- **IChoices.sol** - Interface for Choices modules
- **IPoints.sol** - Interface for Points modules
- **IVotes.sol** - Interface for Votes modules
- **IExecution.sol** - Interface for Execution modules

### Pre-built Modules

#### Choices Modules
- **HatsChoices** - Allows anyone with a specific Hat (NFT role from Hats Protocol) to create a choice, with admin hat management

#### Points Modules
- **ERC20VotesPoints** - Allows voting based on delegated governance token power
- **MerklePoints** - Uses arbitrary merkle trees for determining voting eligibility and power

#### Votes Modules
- **TimedVotes** - Implements time-limited voting periods
- **RubricVotes** - Allows percentage-based voting for each choice (Max-Vote-Per-Choice model)

#### Execution Modules
- **HatsExecution** - Mints Hats Protocol NFTs to arbitrary addresses based on voting results

## Usage

To use Chews Protocol:

1. Select or create the appropriate modules for your use case
2. Deploy each module with the desired parameters
3. Deploy a Contest contract that composes these modules
4. Set the initial contest status (Continuous or Procedural)
5. If Procedural, manage the progression through stages

## Examples

For detailed examples of how to implement and interact with Chews Protocol, please refer to the integration tests:

[Link to integration tests folder]

## Roadmap

Plans for V1 include:

- Improved naming conventions for better clarity
- Standardization of modules for increased reusability
- Better patterns for passing encoded data between modules (particularly from Points to Votes)
- Development of templating and referrer patterns to simplify deployment and configuration
- Comprehensive documentation and tutorials

## Current Limitations

- Initial complexity for developers to learn how components fit together
- Challenges in deploying Contests and configuring module variables
- Ongoing experimentation may lead to architectural changes

## Contributing

Contributions to Chews Protocol are welcome! As this is an experimental project in active development, please reach out to discuss potential contributions.

## License

Chews Protocol is licensed under the [MIT License](LICENSE).
