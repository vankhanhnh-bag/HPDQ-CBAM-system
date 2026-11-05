# Activity Diagram: AD-UC-10 Authenticate and sign in to the platform

swimlane-beta TB

  subgraph ACT\\\\\\\\\\\\\\\["All users"\]

    start@{ shape: sm-circ }

    a1("Provide credentials or OTP")

    a2("Review rejected sign-in")

    a3("Review access denial")

    a4("Access authorized platform functions")

    stop1@{ shape: fr-circ }

    stop2@{ shape: fr-circ }

    stop3@{ shape: fr-circ }

  end

  subgraph SYS\\\\\\\\\\\\\\\["EcoTrace"\]

    s1("Verify user identity (FR-04-01)")

    d1{"Are credentials valid? (FR-04-01)"}

    s2("Reject authentication and deny session (FR-04-01)")

    s3("Read access roles and permissions (FR-04-03)")

    d2{"Is requested access authorized? (FR-04-03)"}

    s4("Deny unauthorized protected access (FR-04-03)")

    s5("Grant secure session with authorized access (FR-04-01)")

  end

  start \--\> a1 \--\> s1 \--\> d1

  d1 \--\>\|"\\\\\\\\\\\\\\\[invalid\]"\| s2 \--\> a2 \--\> stop2

  d1 \--\>\|"\\\\\\\\\\\\\\\[valid\]"\| s3 \--\> d2

  d2 \--\>\|"\\\\\\\\\\\\\\\[not authorized\]"\| s4 \--\> a3 \--\> stop3

  d2 \--\>\|"\\\\\\\\\\\\\\\[authorized\]"\| s5 \--\> a4 \--\> stop1

Reading notes:

* SD-UC-10 loads RBAC roles after authentication, so FR-04-03 (Must; W1 traces it to UC-11) is included only as a cross-use-case access gate, not as a new role-configuration activity. UC-11 remains separate.  
* The sequence also writes both successful and failed sign-in audit events. W1 FR-04-04 only specifies logging system-altering actions, so login logging cannot honestly be claimed covered by FR-04-04. It is explicitly an open question, not added as an untraced system action.