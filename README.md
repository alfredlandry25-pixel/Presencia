# NamePresencia — Project Overview

**Slogan:** *Every Presence, Perfectly Tracked.*

## 1. What This Is

NamePresencia is a facial-recognition-based attendance platform built for university campuses. Instead of manual roll calls, sign-in sheets, or swipe cards, students and staff are recognized automatically as they enter a classroom, lecture hall, or exam venue, and their attendance is logged in real time.

## 2. Problem Statement

Universities today struggle with:
- Manual attendance taking that wastes lecture time
- Proxy attendance (one student signing in for another)
- Delayed or inaccurate attendance records
- No centralized, real-time view of attendance across departments/faculties

## 3. Goals

| Goal | Description |
|---|---|
| Accuracy | Eliminate proxy attendance via biometric facial verification |
| Speed | Recognize and log a student's presence in under 2 seconds |
| Scalability | Support growth from a single pilot classroom to campus-wide, multi-campus deployment |
| Transparency | Give students, lecturers, and administrators real-time visibility into attendance data |
| Privacy & Compliance | Handle biometric data responsibly, with consent and data protection in mind |

## 4. Primary Stakeholders

- **University Administration** — owns policy, compliance, and overall rollout
- **Lecturers / Faculty** — view attendance per class/session, flag anomalies
- **Students** — enroll their face once, get recognized automatically each session
- **IT/Campus Ops** — manage edge devices (cameras) installed in classrooms
- **Engineering Team** — builds and maintains the platform (this documentation set)

## 5. High-Level System Components

NamePresencia is built as a set of independently deployable services:

1. **Web Dashboard** — the interface for admins, lecturers, and students
2. **Edge Agent** — software running on classroom devices/cameras that captures video and forwards frames
3. **Recognition Service** — the ML microservice that matches faces against enrolled embeddings
4. **Attendance Service** — the service of record; owns attendance data, sessions, and business rules

See `ARCHITECTURE.md` for how these fit together, and the individual service READMEs for implementation details.

## 6. Core User Flows

### Enrollment
1. Student registers on the Web Dashboard (or via admin-assisted onboarding)
2. Student captures/uploads reference face images
3. Recognition Service generates and stores a facial embedding tied to the student's ID
4. Student is now "enrolled" and can be recognized by any Edge Agent on campus

### Attendance Capture
1. Lecturer starts a class session on the Web Dashboard (or it auto-starts on schedule)
2. Edge Agent in the room captures video frames of people entering
3. Frames are sent to the Recognition Service, which returns matched student IDs + confidence scores
4. Attendance Service receives matches, validates them against the active session, and records attendance
5. Lecturer/Admin sees attendance populate live on the Web Dashboard

## 7. Non-Functional Requirements

- **Scalability**: horizontally scalable recognition and attendance services; stateless where possible
- **Latency**: end-to-end recognition-to-record latency target < 2s under normal load
- **Availability**: 99.5%+ uptime target for core services during term time
- **Data Privacy**: biometric data encrypted at rest and in transit; retention and consent policies enforced at the platform level
- **Auditability**: every attendance record is traceable to a specific session, device, and confidence score

## 8. Phased Roadmap

| Phase | Scope |
|---|---|
| Phase 1 — Pilot | Single building, 5–10 classrooms, manual session start, core enrollment + recognition + dashboard |
| Phase 2 — Faculty Rollout | Multi-building, scheduled sessions, lecturer/admin roles, reporting |
| Phase 3 — Campus-Wide | Multi-campus support, analytics, integrations with existing Student Information System (SIS) |
| Phase 4 — Scale & Harden | Load-tested for full campus concurrency, SSO, advanced anti-spoofing (liveness detection) |

## 9. Out of Scope (for now)

- Payment/finance integrations
- Full LMS (Learning Management System) replacement
- Mobile native apps (initial rollout is web + edge devices only)

## 10. Related Documents

- `ARCHITECTURE.md` — system architecture and data flow
- `CODE-STANDARD.md` — engineering conventions across all services
- `UI-CONTENT.md` — UI/UX and content guidelines for the Web Dashboard
- `web-dashboard/README.md`
- `edge-agent/README.md`
- `recognition-service/README.md`
- `attendance-service/README.md`
