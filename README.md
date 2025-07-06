# K - Decentralized social media

## Scope
K is a decentralized microblogging platform running on the top of Kaspa network: it allows people to freely express themselves, without any censorship.


## User journey
Here i describe a high-level user journey including basic features of the app:

###  Creating an account
- User A activates the app for the first time and creates its own "K identity";
- This identity is unique in the whole network and represent only this specific user;
- User A locally backup the required data to restore its account, in case he needs to use it on another clinet;
- After creating its identity, User A fund its account by sending a small amount of KAS from his personal wallet, in order to pay for fees;
- User A broadcast the first message in K network, by informing everyone about his skills, interest, topics of choice, etc; in this way, he's promoting its new activity on the network.

###  Posting the first message
- User A posts his first message on K network, to let everyone know how exciting decentralization and Kaspa are.

###  Following/unfollowing and supporting/unsupporting a user
- User B, who already activated a K account last month, receive the broadcast message from User A and decides to follow him;
- User B activates the "following" process: everytime User A posts something new, User B is alerted and visualize this new content on his K home page;
- User B decides that User A contents are interesting, therefore he chooses to support him;
- When activating the supporting process, User B is sending a certain amount of KAS to User A;
- User A is notified: now he knows User B is supporting him; in addition, everyone else in the network knows User A is supported by User B; 
- In the meantime, User C, another long time K user, received User A broadcast and start following and receiving notifications when User A posts something;
- But User C doesn't like User A contents, therefore he doesn't activate the supporting process and, after few days, he also unfollow User A.

###  Commenting user contents
- User A can setup his own account to:
  - Allow all users to comment his posts;
  - Allow only user who support him to comment his posts;
- User B can freely comment User A content (whether he support User A or not);
- Depending on User A settings (above), User A receive the comment, gets notified about the comment, read the comment or ignore it (not visualizing it).

###  Mentioning other users in posts or comment
- User A can setup his own account to:
  - Allow all users to mention him;
  - Allow only user who support him to to mention him;
- User B can freely mention User A on its posts or comments (whether he support User A or not);
- Depending on User A settings (above), User A receive the mention, gets notified about the mention, read the post/comments including the mention or ignore it (not visualizing it).

###  Reposting/quoting user contents
- User A can setup his own account to:
  - Gets notified if someone repost (forwarding a message, adding considerations) its content;
  - Gets notified if someone quotes (forwarding a message) its content;
- User B can freely repost and quote a User A content in it's own profile (whether he support User A or not);
- Depending on User A settings (above), User A receive the repost/quote, gets notified about the repost/quote, read the repost/quote or ignore it (not visualizing it).

###  Upvoting/downvoting user contents
- User A can setup his own account to:
  - Gets notified if someone upvote/downvote its content;
  - Gets notified if someone quotes upvote/downvote its content;
- User B can freely upvote/downvote a User A content (whether he support User A or not);
- Depending on User A settings (above), User A receive the upvote/downvote, gets notified about the upvote/downvote, see the upvote/downvote or ignore it (not visualizing it).

###  User history
- When User B starts following or supporting (TBD) User A, he'll receive all past contents posted by User A in the past.

###  Blocking users
- Even if User B follow or support User A, User A can freely decide to block User B, blocking notifications related to all main actions (comments, mentions, retweek, quotes);
- User B can freely mention User A and comment, retweek and quote User A contents but User A will never be notified about these actions.

###  Endorsing (suggesting) users
- User A like User B so much that he feels like User B deserve an endorsement, due to the value of his contents;
- User A broadcast an "endorsement" message to all his supporting users, suggesting to follow and support User B;
- All users supporting User A receive a notification which invites them to support User B.


## Main technical requirements
- The app should work both locally (on a PC, for a single user) and hosted (exposed on the internet, for multiple users);
- For a single user, the only requirement to run this app is a local Kaspa node running on mainnet (to be completely decentralized).

## Potential architecture - Communication scheme

###  Creating an account
- User A broadcast the first message in K network, by informing everyone about his skills, interest, topics of choice, etc; in this way, he's promoting its new activity on the network.

```mermaid
sequenceDiagram
    actor A as Alice (front-end)
    A->>Alice Kaspa node: I'm Alice, follow me!
    Alice Kaspa node-->>Bob's Kaspa node: I'm Alice, follow me!    
    Bob's Kaspa node->>Bob's indexer: I'm Alice, follow me!
    create actor B as Bob (front-end)
    Bob's indexer->>B: I'm Alice, follow me!
    create participant Carl's Kaspa node
    Alice Kaspa node-->>Carl's Kaspa node: I'm Alice, follow me!
    create participant Carl's indexer
    Carl's Kaspa node->>Carl's indexer: I'm Alice, follow me!
    create actor C as Carl (front-end)
    Carl's indexer->>C: I'm Alice, follow me!
```

**Protocol specifications**

| Action | Parameters | Body |
|--------|------------|------|
| broadcast | --- | message |

Payload example: k:1:post:sender_pubkey:sender_signature:body

###  Posting the first message
- User A posts his first message on K network, to let everyone know how exciting decentralization and Kaspa are.

```mermaid
sequenceDiagram
    actor A as Alice (front-end)
    A->>Alice Kaspa node: I love decentralization!
    Alice Kaspa node->>Alice's indexer: I love decentralization!
    Alice's indexer-->>A: I love decentralization!
```

**Protocol specifications**

| Action | Parameters | Body |
|--------|------------|------|
| post | --- | message |

Payload example: k:1:post:sender_pubkey:sender_signature:body


###  Following a user
- User B activates the "following" process: everytime User A posts something new, User B is alerted and visualize this new content on his K home page;

 ```mermaid
sequenceDiagram
    actor B as Bob (front-end)
    B->>Bob's indexer: I want to follow Alice!
    actor A as Alice (front-end)
    A->>Alice Kaspa node: I love Kaspa!
    Alice Kaspa node-->>Bob Kaspa node: I love Kaspa!
    Bob Kaspa node->>Bob's indexer: I love Kaspa!
    Bob's indexer->>B: I love Kaspa!
```

###  Supporting a user
- When activating the supporting process, User B is sending a certain amount of KAS to User A and User A is notified.

 ```mermaid
sequenceDiagram
    actor B as Bob (front-end)
    B->>Bob Kaspa node: I support Alice!
    Bob Kaspa node-->>Alice Kaspa node: I support Alice!
    Alice Kaspa node->>Alice's indexer: I support Alice!
    actor A as Alice (front-end)
    Alice's indexer->>A: I support Alice!
```

**Protocol specifications**

| Action | Parameters | Body |
|--------|------------|------|
| support | recipient pubkey | message |

Payload example: k:1:support:sender_pubkey:sender_signature:body


###  Commenting user contents
- User B can freely comment User A content (whether he support User A or not).

```mermaid
sequenceDiagram
    actor A as Alice (front-end)
    participant Alice's indexer
    A->>Alice Kaspa node: This is my new post!
    Alice Kaspa node-->>Bob Kaspa node: This is my new post!
    Bob Kaspa node->>Bob's indexer: This is my new post!
    actor B as Bob (front-end)
    Bob's indexer->>B: This is my new post!
    B->>Bob Kaspa node: Here is my comment!
    Bob Kaspa node-->>Alice Kaspa node: Here is my comment!
    Alice Kaspa node->>Alice's indexer: Here is my comment!
    Alice's indexer->>A: Here is my comment!
```

**Protocol specifications**

| Action | Parameters | Body |
|--------|------------|------|
| comment | post ID (tx ID) | message |

Payload example: k:1:comment:sender_pubkey:sender_signature:body


###  Replying other users comments
TBD

**Protocol specifications**

| Action | Parameters | Body |
|--------|------------|------|
| reply | comment ID (tx ID) | message |

Payload example: k:1:reply:sender_pubkey:sender_signature:body


###  Mentioning other users in posts or comment
- User B can freely mention User A on its posts or comments (whether he support User A or not).

```mermaid
sequenceDiagram
    actor B as Bob (front-end)
    B->>Bob Kaspa node: I mention @Alice on this post!
    Bob Kaspa node-->>Alice Kaspa node: I mention @Alice on this post!
    Alice Kaspa node->>Alice's indexer: I mention @Alice on this post!
    actor A as Alice (front-end)
    Alice's indexer->>A: I mention @Alice on this post!
```

**Protocol specifications**

| Action | Parameters | Body |
|--------|------------|------|
| post | user addresses[] | message |
| comment | post ID (tx ID), user addresses[] | message |
| reply | comment ID (tx ID), user addresses[] | message |

Payload examples:
- k:1:post:sender_pubkey:sender_signature:body
- k:1:comment:sender_pubkey:sender_signature:body
- k:1:reply:sender_pubkey:sender_signature:body


###  Reposting/quoting user contents
- User B can freely repost and quote a User A content in it's own profile (whether he support User A or not).

 ```mermaid
sequenceDiagram
    actor A as Alice (front-end)
    participant Alice's indexer
    A->>Alice Kaspa node: This is my new post!
    Alice Kaspa node-->>Bob Kaspa node: This is my new post!
    Bob Kaspa node->>Bob's indexer: This is my new post!
    actor B as Bob (front-end)
    Bob's indexer->>B: This is my new post!
    B->>Bob Kaspa node: I repost/quote Alice's post!
    Bob Kaspa node-->>Alice Kaspa node: I repost/quote Alice's post!
    Alice Kaspa node->>Alice's indexer: I repost/quote Alice's post!
    Alice's indexer->>A: I repost/quote Alice's post!
```

**Protocol specifications**

| Action | Parameters | Body |
|--------|------------|------|
| repost | post ID (tx ID) | --- |
| quote | post ID (tx ID) | message |

Payload examples:
- k:1:repost:sender_pubkey:sender_signature:body
- k:1:quote:sender_pubkey:sender_signature:body

###  Upvoting/downvoting user contents
- User B can freely upvote/downvote a User A content (whether he support User A or not).

```mermaid
sequenceDiagram
    actor A as Alice (front-end)
    participant Alice's indexer
    A->>Alice Kaspa node: This is my new post!
    Alice Kaspa node-->>Bob Kaspa node: This is my new post!
    Bob Kaspa node->>Bob's indexer: This is my new post!
    actor B as Bob (front-end)
    Bob's indexer->>B: This is my new post!
    B->>Bob Kaspa node: I like Alice's post!
    Bob Kaspa node-->>Alice Kaspa node: I like Alice's post!
    Alice Kaspa node->>Alice's indexer: I like Alice's post!
    Alice's indexer->>A: I like Alice's post!
```

**Protocol specifications**

| Action | Parameters | Body |
|--------|------------|------|
| vote | post ID (tx ID), up/down | --- |
| vote | comment ID (tx ID), up/down | --- |

Payload example: k:1:vote:sender_pubkey:sender_signature:body

###  User history
- When User B starts following or supporting (TBD) User A, he'll receive all past contents posted by User A in the past.

TBD

###  Blocking users
- Even if User B follow or support User A, User A can freely decide to block User B, blocking notifications related to all main actions (comments, mentions, retweek, quotes).

 ```mermaid
sequenceDiagram
    actor A as Alice (front-end)
    A->>Alice's indexer: I want to block Bob!
    participant Alice Kaspa node
    participant Bob Kaspa node
    actor B as Bob (front-end)
    B->>Bob Kaspa node: I'm trying to comment Alice's post!
    Bob Kaspa node-->>Alice Kaspa node: I'm trying to comment Alice's post!
    Alice Kaspa node->>Alice's indexer: I'm trying to comment Alice's post!
```

###  Endorsing (suggesting) users
- User A broadcast an "endorsement" message to all his supporting users, suggesting to follow and support User B;

```mermaid
sequenceDiagram
    actor A as Alice (front-end)
    A->>Alice Kaspa node: Hey guys, please support Bob!
    Alice Kaspa node-->>Carl's Kaspa node: Hey guys, please support Bob!
    Carl's Kaspa node->>Carl's indexer: Hey guys, please support Bob!
    create actor C as Carl (front-end)
    Carl's indexer->>C: Hey guys, please support Bob!
    create participant Dereck's Kaspa node
    Alice Kaspa node-->>Dereck's Kaspa node: Hey guys, please support Bob!
    create participant Dereck's indexer
    Dereck's Kaspa node->>Dereck's indexer: Hey guys, please support Bob!
    create actor D as Dereck (front-end)
    Dereck's indexer->>D: Hey guys, please support Bob!
```
