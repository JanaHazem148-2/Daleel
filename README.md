# Daleel — Wheelchair Accessibility Map System                                                                                                                      #"دليل" (Daleel) means guide in Arabic — a digital guide empowering wheelchair users and people of determination to navigate the world independently.
---

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Solution](#solution)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Modules](#modules)
- [User Roles](#user-roles)
- [Database Schema](#database-schema)
- [Sequence Flows](#sequence-flows)
- [Non-Functional Requirements](#non-functional-requirements)
- [Development Plan](#development-plan)
- [Expected Outcomes](#expected-outcomes)

---

## Overview

Daleel is a web-based platform designed as a **"Google Maps for wheelchair accessibility"** — a centralized, community-driven system that maps, rates, and routes wheelchair-accessible locations. It combines interactive mapping, community contributions, emergency assistance, and smart analytics to improve the daily mobility and independence of wheelchair users and people of determination.

---

## Problem Statement

Wheelchair users and people of determination face significant barriers when navigating cities and public/private facilities:

- **Lack of accurate information** — No reliable, centralized system documents accessibility features (ramps, elevators, restrooms) across locations.
- **Gap between expectation and reality** — Many facilities claim to be accessible but fall short (broken elevators, missing ramps, inadequate restrooms).
- **Dependence on trial and error** — Users must physically visit locations to verify accessibility, wasting time and leading to frustration and social isolation.

These challenges restrict freedom of movement, reduce independence, and negatively impact social, psychological, and economic well-being.

---

## Solution

A web platform that allows users to:

- Discover accessible locations on an interactive map.
- Contribute real-world reviews, photos, and accessibility ratings.
- Plan wheelchair-friendly routes avoiding stairs and obstacles.
- Request emergency help with automatic location sharing.
- Receive smart recommendations based on usage patterns.
- Contact support and receive notifications about accessibility changes.

---

## Key Features

### 🗺️ Interactive Map
- Displays nearby locations categorized as **Accessible**, **Partially Accessible**, or **Not Accessible**.
- Visual markers for ramps, functional elevators, accessible restrooms, and stair-free entrances.
- Supports zoom, pan, and satellite views.

### 🧭 Accessible Route Planning
- Calculates wheelchair-friendly routes avoiding stairs, narrow paths, and steep slopes.
- Integrates with **Google Maps API** for directions.
- Filters routes by user accessibility preferences.
- Provides voice-guided navigation with real-time re-routing around obstacles.

### 👥 Community Contributions
- Users can add locations, submit photos, and rate accessibility.
- Contribution badges: **Top Contributor**, **Photo Master**, **Review Master**.
- Top Contributors can publish content directly without admin approval.
- Admin verification required for standard submissions.

### 🚨 Emergency Assistance
- One-tap **Request Help** button with live GPS location sharing.
- Dispatches nearest available emergency assistant.
- Notifies registered security staff or pre-saved emergency contacts.
- Automatic escalation to local emergency services (e.g., 122 in Egypt) if no assistant is available.

### 🎫 Contact & Support
- Multi-language support ticket system (Arabic and English).
- Automatic agent assignment based on availability and specialty.
- Email and in-app notifications for ticket status.

### 📊 Data Analytics & Visualization
- Heatmaps showing high and low accessibility areas.
- Dashboards for administrators and authority representatives.
- Statistics exportable in CSV, Excel, or PDF formats.

### 🔔 Smart Notifications & Alerts
- Real-time alerts for accessibility changes (e.g., elevator outages).
- Multi-channel delivery: Email, SMS, Push, In-App.

### 🌐 Multi-Language & Accessibility Support
- Arabic (RTL) and English (LTR) with auto-layout switching.
- ARIA labels, screen reader support, full keyboard navigation.
- Large-text options and voice assistance for visually impaired users.

---

## System Architecture

The system follows a **layered service-oriented architecture** with the following top-level components:

| Component | Responsibility |
|---|---|
| `Controller` | Central request routing and service coordination |
| `AuthenticationManagement` | Login, logout, session management, password verification |
| `MapService` | Route search, calculation, map rendering |
| `CommunityManagement` | Contributions, participation, badge management |
| `EmergencyManagement` | Emergency requests, dispatch, tracking |
| `SupportService` | Support tickets, agent assignment |
| `NotificationService` | Multi-channel notification delivery |

Each component implements a well-defined interface (e.g., `IMapService`, `IEmergencyManagement`) and communicates through the `Controller`, which manages data flow and cross-service coordination.

---

## Modules

### 1. Authentication Management
Handles user login, session lifecycle, and credential validation. Integrates with `RoleManagement` and `ProfileManagement` to load permissions and preferences on login.

### 2. Role & Profile Management
Supports four system roles with distinct permissions:

| Role | Access Level |
|---|---|
| Regular User | Map, routes, reviews, contributions, emergency help |
| Business Owner | Manage own location's accessibility data, respond to reviews |
| Admin | Full system access, account management, content moderation |
| Authority Representative | Read-only analytics and export for policy use |

### 3. Map Management
Renders the interactive map, manages layers and markers, and displays accessibility features on routes.

### 4. Route Management
Calculates, optimizes, and filters wheelchair-accessible routes. Saves routes to user profiles and records location history.

### 5. Location Management
Validates and stores location data. Tracks current user location via GPS and maintains a location history per user.

### 6. Community Participation & Badges
Tracks user contributions (reviews, photos, ratings). Awards badges based on point thresholds. Notifies admins of high-level badge achievements.

### 7. Emergency Assistance
Accepts emergency requests, dispatches the nearest available assistant, coordinates with security staff, and escalates to external emergency services when needed.

### 8. Contact Support
Manages the full support ticket lifecycle: creation, agent assignment (by availability and specialty), messaging, and resolution. Supports multilingual tickets.

### 9. Notification & Alert Management
Creates, routes, and tracks delivery of notifications and system alerts across email, SMS, push, and in-app channels.

### 10. Report Management
Allows users to submit obstacle reports and correction requests. Admin verification is required before map updates are applied.

### 11. Language Support
Validates selected languages, falls back to default, and provides translation lookups for all system content.

---

## User Roles

```
Regular User ──────────────────────────────────────────────────────────┐
  ├── Search and plan accessible routes                                 │
  ├── Submit reviews, photos, and ratings                               │
  ├── Earn and view contribution badges                                 │
  ├── Request emergency help                                            │
  ├── Contact support and track tickets                                 │
  └── Manage profile and notification preferences                       │
                                                                        │
Business Owner ────────────────────────────────────────────────────────┤
  ├── Register and update location accessibility info                   │
  └── Respond to user reviews                                           │
                                                                        │
Admin ─────────────────────────────────────────────────────────────────┤
  ├── Approve/suspend accounts                                          │
  ├── Moderate community content and reviews                            │
  ├── Verify and process reports                                        │
  └── Access full analytics dashboard                                   │
                                                                        │
Authority Representative ──────────────────────────────────────────────┘
  ├── View anonymized regional accessibility data
  └── Export reports for policy and urban planning use
```

---

## Database Schema

The system database contains **40+ entities** across the following domains:

| Domain | Key Tables |
|---|---|
| Users & Auth | `USER`, `SESSION`, `USER_PROFILE`, `ROLE`, `ROLE_ASSIGNMENT`, `PERMISSION` |
| Map & Location | `LOCATION`, `ACCESSIBILITY_FEATURE`, `LOCATION_HISTORY`, `MAP_LAYER`, `MAP_MARKER` |
| Routing | `ROUTE`, `ROUTE_POINT` |
| Community | `CONTRIBUTION`, `COMMUNITY_PARTICIPATION`, `PARTICIPATION_ACTIVITY`, `CONTRIBUTION_BADGE`, `BADGE_TYPE` |
| Reviews & Reports | `REVIEW`, `REPORT`, `REPORT_TYPE` |
| Emergency | `EMERGENCY_REQUEST`, `EMERGENCY_RESPONSE`, `EMERGENCY_ASSISTANT`, `EMERGENCY_CONTACT` |
| Support | `SUPPORT_TICKET`, `TICKET_MESSAGE`, `SUPPORT_AGENT` |
| Notifications | `NOTIFICATION`, `NOTIFICATION_DELIVERY`, `NOTIFICATION_TYPE`, `ALERT`, `ALERT_RECIPIENT` |
| Internationalization | `LANGUAGE`, `TRANSLATION` |

---

## Sequence Flows

The system documents five core use case sequence flows:

### Use Case 1 — Manage Authentication
Login → verify credentials → load roles & permissions → load profile → create session. Handles account lockout after 5 failed attempts.

### Use Case 2 — Manage Participation
Submit contribution → validate & sanitize → save to DB → update participation points → check badge eligibility → award badges → notify user.

### Use Case 3 — Manage Map
Search route → validate locations → calculate accessible route via Google Maps API → filter by accessibility features → save route & waypoints → display on map → record location history.

### Use Case 4 — Request Emergency Help
Press emergency button → validate location → classify emergency type → dispatch nearest assistant → notify security staff → send multi-channel notifications to user. If no assistant is available, automatically escalates to external emergency services.

### Use Case 5 — Manage Contact Support
Submit support request → validate language → create ticket → assign available agent by specialty → notify user and agent via email and in-app → track ticket lifecycle.

---

## Non-Functional Requirements

| Category | Requirement |
|---|---|
| **Performance** | Map loads within 3 seconds on a 10 Mbps connection; supports 500+ concurrent users |
| **Security** | AES-256 encryption for sensitive data; HTTPS (SSL) for all transmissions; RBAC enforced across all modules |
| **Availability** | 99% monthly uptime; automatic daily backups; full recovery within 10 minutes of failure |
| **Usability** | WCAG-compliant; full keyboard navigation; screen reader support; ARIA labels on all elements |
| **Localization** | Arabic and English with automatic RTL/LTR layout switching |
| **Data Integrity** | Encrypted backups with version control; admin-verified before map updates |

---

## Development Plan

The project is organized across **5 sprints** with **46 functions** tracked across 11 modules.

### Sprints & Milestones

| Sprint | Focus | Milestone |
|---|---|---|
| Sprint 1 | Authentication module | M1 — Auth complete |
| Sprint 2 | Contribution & Participation | M2 — Community complete |
| Sprint 3 | Location, Map, Route modules | M3/M4/M5 — Map & routing complete |
| Sprint 4 | Emergency, Notification, Review modules | M6/M7/M8 — Emergency & notifications complete |
| Sprint 5 | Integration, testing, final delivery | M9/M10 — System integration & release |

### Critical Path

```
Authentication → Map → Location → Route → Emergency → Notification
```

Delays in any step on the critical path directly affect the project delivery date.

### Estimation Summary

| Module | Story Points | Duration |
|---|---|---|
| Authentication | 21 | 2 days |
| Contributions | 14 | 2 days |
| Community Participation | 16 | 2 days |
| Contact Support | 14 | 2 days |
| Location Management | 16 | 2 days |
| Review Management | 19 | 2 days |
| Route Management | 29 | 3 days |
| Report Management | 11 | 1 day |
| Map Management | 21 | 2 days |
| Notification Management | 11 | 1 day |
| Emergency Help | 21 | 2 days |

---

## Expected Outcomes

### Social Impact
- Greater independence and mobility for wheelchair users and people of determination.
- Reduced frustration and social isolation; stronger community participation.
- Increased awareness among businesses about the value of accessibility.

### Economic Impact
- Increased foot traffic to accessible places, boosting revenue for accessibility-investing businesses.
- Competitive incentive for service providers to improve accessibility ratings.

### For People of Determination
- Easier access to essential services, leisure, education, and government facilities.
- Enhanced dignity and independence, reducing reliance on others.
- A stronger sense of belonging and equal opportunity in society.

---

*Daleel — navigating the world, together.*
