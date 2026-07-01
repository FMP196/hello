# stateDiagram-v2
    [*] --> Draft: User defines service<br/>(auth, deps, schema, metadata)

    Draft --> Verifying: Click "Add"

    state Verifying {
        direction LR
        [*] --> SchemaCheck
        SchemaCheck --> Connectivity: pass
        Connectivity --> AuthHandshake: pass
        AuthHandshake --> ContractCheck: pass
        ContractCheck --> [*]: pass
    }

    Verifying --> Draft: any gate fails<br/>(actionable error)
    Verifying --> Staged: all gates pass

    Staged --> Certifying: run synthetic tests
    Certifying --> Staged: user rejects output
    Certifying --> PendingApproval: user confirms output<br/>(pairs become golden tests)

    PendingApproval --> Published: approved<br/>(auto / human gate)
    PendingApproval --> Staged: changes requested

    Published --> Monitoring: scheduled health +<br/>contract + golden checks
    Monitoring --> Quarantined: repeated failure /<br/>contract drift
    Quarantined --> Staged: re-certify
    Published --> Deprecated: retire
    Deprecated --> [*]

