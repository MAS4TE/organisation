# Overview Use Cases

This document describes user interactions with a **community storage
market system**, where households (*prosumers*) can buy or sell
**storage capacity** in a shared community market.

The system is accessed through a conversational app interface powered by
a **Large Language Model (LLM)**. Prosumers simply provide natural
language input about their plans (e.g., *"I'll need storage for next
week"* or *"I'm away for a week"*). Based on this, the system
autonomously manages buying or selling storage capacity, using
predictive models, battery models, and the market framework (ASSUME).
After completing an action, the system explains to the user what was
done and why.

Two scenarios illustrate these capabilities:

1.  **Buying storage capacity** for a household without a battery system
    during a week of high energy demand.

2.  **Selling unused storage capacity** from a household with a battery
    system during a week-long holiday.

## Scenario 1: Buying Storage Capacity for a Week of Renovations

**Context**

A household plans a week of evening renovations that require significant
energy use. They do not own a personal battery system but want to
reserve **storage capacity** in the community market. This will allow
them to store cheap off-peak energy during the day and use it during the
evenings when their solar panels are not producing energy.

**User Story**

*As a prosumer, I want the system to autonomously arrange storage
capacity in the community market for a whole week while I'm renovating,
so I can shift energy use and cover my increased demand without worrying
about technical details or market transactions.*

**Use Case: Autonomous Multi-Day Storage Purchase**

-   **Actors**:

    -   Prosumer (Household)

    -   LLM Interface

    -   Trading Engine/Agent

    -   Market Framework (ASSUME)

-   **Goal**:\
    Automatically reserve storage capacity in the community market for a
    week of renovations based on user-provided context.

-   **Preconditions**:

    -   The prosumer has an active account and app installed.

**Main Flow**

1.  The prosumer opens the app and says:\
    *"I'll be renovating next week during the evenings and need storage
    capacity since my solar panels won't generate power at night."*

2.  The LLM interprets this request and autonomously determines that
    additional storage capacity is needed for seven days.

3.  The LLM sends the request to the Trading Engine.

4.  The Trading Engine:

    -   Estimates the household's daily storage needs for evening
        renovations (e.g., 8kWh/day).

    -   Checks market availability and pricing for storage capacity over
        the next week.

    -   Reserves 56kWh of total storage capacity (8kWh/day × 7 days) at
        €0.07/kWh.

5.  The Market Framework (ASSUME) processes the trade and confirms the
    reservation.

6.  The LLM notifies the prosumer afterwards:\
    *"I've reserved 8kWh of storage capacity per day for next week at
    €0.07/kWh to support your renovation plans."*

**Alternative Flows**

-   **Partial Market Availability**:

    -   If only part of the week is covered, the LLM informs the user:\
        *"I was able to reserve storage for 5 of the 7 days. I'll
        continue monitoring the market for additional availability."*

-   **High Price Condition**:

    -   If prices exceed a threshold, the LLM may still act but
        explains:\
        *"I reserved storage at €0.12/kWh due to limited availability
        during peak hours."

## Scenario 2: Selling Unused Storage Capacity During a Week-Long
Holiday

**Context**

A household owns a personal battery storage system that they typically
use to store excess solar energy for later use. However, they are
leaving for a one-week holiday and do not need their storage capacity
during this period. They want the system to autonomously sell this
unused capacity in the community market so others can use it, and they
can earn income from their idle battery.

**User Story**

*As a prosumer, I want the system to autonomously sell my unused storage
capacity in the community market while I'm on holiday for a week, so I
can generate income without managing the details myself.*

**Use Case: Autonomous Multi-Day Storage Sale**

-   **Actors**:

    -   Prosumer (Household)

    -   LLM Interface

    -   Trading Engine

    -   Market Framework (ASSUME)

-   **Goal**:\
    Automatically make the household's storage capacity available in the
    community market for others to use during their week-long absence.

-   **Preconditions**:

    -   The prosumer owns a battery system with available capacity.

    -   The community market has demand for storage capacity.

**Main Flow**

1.  The prosumer opens the app and says:\
    *"We're going on holiday for a week and won't need our battery
    storage."*

2.  The LLM interprets this and autonomously determines that the
    household's storage capacity can be made available for others.

3.  The LLM sends the request to the Trading Engine.

4.  The Trading Engine/Agent:

    -   Confirms the household's storage capacity availability (e.g.,
        12kWh/day).

    -   Queries current market demand and pricing.

    -   Sells 84kWh total storage capacity (12kWh/day × 7 days) at
        €0.09/kWh.

5.  The Market Framework (ASSUME) processes the trade and matches the
    seller with buyers.

6.  The LLM notifies the prosumer afterwards:\
    *"Your storage capacity has been made available on the community
    market for the next 7 days. You'll earn €0.09/kWh for a total of
    €7.56 over the week."*

**Alternative Flows**

-   **No Immediate Buyers**:

    -   The LLM informs the user:\
        *"No buyers are currently available. I'll continue monitoring
        demand and sell the capacity once there's interest."*

-   **Partial Sale**:

    -   If only part of the storage capacity is sold:\
        *"We were able to sell 70% of your available storage capacity.
        I'll keep offering the remainder."*

## Summary

These use cases demonstrate how the system supports prosumers in
participating in a **community storage market**. By enabling autonomous
buying and selling of storage capacity over multiple days, the system
helps households optimize their energy strategies and contribute to a
shared, efficient energy ecosystem.
