# Diagrammes de séquence
## Create and participate to an event
```mermaid
sequenceDiagram
    actor L as Lena
    actor P as Paul
    participant A as Application
    participant QR as Quest and Reward Services
    L ->> A: Create a new Event
    A ->> A: Validate the Event
    A ->> P: Event now appears available
    P ->> A: Choose to participate in the Event
    A ->> A: Validate the participation
    A -->> P: Gives a QR code to Paul
    A ->> L: Notify Lena of Paul's participation
    alt Event is finally happening
        L ->> P: Scan Paul's QR code
        L ->> A: Validate Paul's participation
        A ->> QR: Progress quests and successes for Paul and Lena
        QR ->> A: Quests and successes are updated
        A ->> P: Reward Paul for his participation
        A ->> L: Reward Lena for her event

    else Event is cancelled
        A->>A: Cancel the Event
        A->>P: Notify Paul of the cancellation
        A->>L: Notify Lena of the cancellation
    end
```

## Get experience points
```mermaid
sequenceDiagram
    actor L as Léna(user)
    participant A as Application
    participant QM as Quest Manager
    participant R as Reward Manager
    L ->> A: Do quest action
    A ->> QM: Léna has done an action that progresses a quest
    QM ->> QM: updateQuestProgression
    alt Léna has finished quest
        QM ->> QM: updateQuestStatus
        QM ->> A: Quest finished
        A ->> R: Request reward
        R ->> R: Calculate reward
        R ->> A: Reward experience to user
        A ->> L: Notify Quest finished and rewards xp
    else Léna has not finished quest
        QM ->> A: Quest not finished
        A ->> L: Notify quest progression
    end
```

## Play game and do quests and successes
```mermaid
sequenceDiagram
    actor L as Léna(user)
    participant A as Application
    participant QM as Quest Manager
    participant R as Reward Manager
    L ->> A: Do quest action
    A ->> QM: Léna has done an action that progresses a quest
    QM ->> QM: updateQuestProgression
    alt Léna has finished quest
        QM ->> QM: updateQuestStatus
        QM ->> A: Quest finished
        A ->> R: Request reward
        R ->> R: Calculate reward
        R ->> A: Reward experience to user
        A ->> L: Notify Quest finished and rewards xp
    else Léna has not finished quest
        QM ->> A: Quest not finished
        A ->> L: Notify quest progression
    end
```

## Quest attribution
```mermaid
sequenceDiagram
    actor L as Léna(user)
    participant Application
    participant Quest Manager
    Quest Manager ->> Quest Manager: UpdateDailyQuests(Léna)
    Quest Manager ->> Quest Manager: UpdateWeeklyQuests(Léna)
    L ->> Application: Login
    Application ->> Quest Manager: GetQuests(Léna)
    alt user logs in for the first time today
        Quest Manager ->> Quest Manager: GenerateDailyQuests(Léna)
        Quest Manager ->> Application: ReturnDailyQuests(Léna)
    else user has already logged in today
        Quest Manager ->> Application: ReturnDailyQuests(Léna)
    end
    Application ->> L: DisplayDailyQuests(Léna)
    alt user logs in for the first time this week
        Quest Manager ->> Quest Manager: GenerateWeeklyQuests(Léna)
        Quest Manager ->> Application: ReturnWeeklyQuests(Léna)
    else user has already logged in this week
        Quest Manager ->> Application: ReturnWeeklyQuests(Léna)
    end
    Application ->> L: DisplayWeeklyQuests(Léna)
```
## Subscribe and donate
```mermaid
sequenceDiagram
    actor L as Lena
    participant A as Application
    participant S as Subscription
    participant P as Payment Method

    L->>A: Wants to subscribe
    A-->L: Shows subscription options
    L->>A: Chooses subscription method
    A-->L: Redirect to payment method
    L->>P: Enters payment details
    P-->>A: Validates payment details
    A->>S: Create(Lena)
    S-->>A: Subscription created
    A-->>L: Notifies Lena is now a subscriber
    A->>L: Provides points and perks of subscriber
```

## User interacts with store
```mermaid
sequenceDiagram
    actor L as Lena(user)
    participant S as Store
    participant P as Payment Service
    L->>L : has earned points to spend in the store
    L->>S : tries to buy a prize available in the store
    S->>S: checks if the Lena has enough points to buy the prize
    alt Lena has enough points to buy the prize
        S->>S : Store attributes the prize to the user
        S-->>L : Notifies Lena that the prize has been bought
        L->>L : Can have access to the asset in their profile page
    else Lena does not have enough points to buy the prize
        S-->>L : Store suggests Lena to buy more points to buy the prize
        alt Lena wants to buy more points
            L->>P : Buy points
            P-->>L : Notifies Lena that the points have been bought
            L->>S : Buy the prize
            S->>S : Store attributes the prize to the user
            S-->>L : Notifies Lena that the prize has been bought
        else Lena does not want to buy more points
        S-->>L : Notifies Lena that the prize has not been bought
        L->>L : Cannot have access to the asset in their profile page
        end
    end
```
