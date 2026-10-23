
  MiniGit Version Control Engine

MiniGit is a command-line version control system built from scratch in Python. It implements a content-addressable storage model and an object directed acyclic graph (DAG) to track, snapshot, and reconstruct file system states. 

 Codebase Architecture
 
minigit_project/
├── minigit/
│   ├── index.py          # Tracks staging area matrix and index states
│   ├── cli.py            # Command Line Interface gateway entry point
│   ├── repository.py     # Manages .minigit repository initialization and I/O
│   ├── objects/          # Content-addressable object database
│   │   ├── base.py       # Base class for hashed object parsing
│   │   ├── blob.py       # Handles compressed file content snapshots
│   │   ├── tree.py       # Models structural directory hierarchies
│   │   ├── commit.py     # Stores commit metadata and parent graph links
│   │   └── tag.py        # Implements persistent reference tags
│   └── commands/         # Core VCS algorithmic execution logic
│       ├── init.py       # Initializes local repository tracking environment
│       ├── add.py        # Computes object hashes and stages files in the index
│       ├── commit.py     # Writes immutable tree states to the object database
│       ├── checkout.py   # Reconstructs working directory from a commit tree hash
│       ├── diff.py       # Compares data deltas between file versions
│       ├── merge.py      # Implements pointer merging algorithms
│       ├── log.py        # Walks parent commit hashes to display commit history
│       └── status.py     # Scans working directory for modified/untracked changes
└── setup.py              # Packaging configuration script

Use code with caution.

  Core Systems Logic Implemented

  Content-Addressable Storage (CAS): Separates file contents from system filenames. Every file (Blob), directory snapshot (Tree), and historical state (Commit) is indexed using a cryptographic hashing algorithm as its unique address key.
  Object Directed Acyclic Graph (DAG): Commits are modeled sequentially inside a DAG structure, where each commit object preserves an immutable cryptographic signature pointing directly back to its parent node.
  Binary Stream Serialization: Handles custom serialization algorithms to encode live runtime objects into disk-persistent files and deserialize them back into object graphs during lookups.
  Staging Index Mechanism: Implements a localized state tracking index (index.py) that caches workspace changes, allowing for file isolation before committing to the permanent database.

  Core CLI Commands Natively Implemented

  minigit init : Initializes a new local object tracking directory.
  minigit add <file> : Hashes file content, writes it to the object database, and updates the staging index.
  minigit commit -m "message" : Bundles staged index pointers into a directory Tree object and seals it into a Commit node.
  minigit status : Compares the current working directory, index state, and HEAD pointer to identify untracked or modified files.
  minigit log : Traverses the ancestral commit chain backward from the current HEAD to display historical tracking entries.
  minigit checkout <commit-hash> : Updates the physical workspace directory structure to precisely match a prior commit point.
