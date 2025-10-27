# Case Study: Real-Time Industrial Control System (Summer 2025 Internship)

> **Disclaimer:** This repository is a case study for a project I developed during my Summer 2025 internship under a Non-Disclosure Agreement (NDA). It **does not contain any proprietary source code or confidential information.** The purpose is to document my personal contributions, technical architecture, and the challenges I solved.

---

## 🚀 Project Overview

The DCS Gateway is a web-based, real-time industrial control system designed for managing chemical reactors in a manufacturing environment. I served as the **sole frontend developer** in a two-person engineering team, responsible for architecting and delivering the entire client-side application from scratch.

The system provides a unified interface for engineers to monitor live data, control reactor parameters remotely, and analyze historical performance, replacing a manual, on-site-only workflow.

---

## 🛠️ Tech Stack

- **Frontend:** React 18, Zustand (for global state), WebSockets, Recharts (for data visualization)
- **Backend:** Go (Fiber Framework), PostgreSQL, Redis
- **DevOps & Deployment:** Docker, AWS ECS, GitHub Actions for CI/CD

---

## ✨ Key Features & My Contributions

As the sole frontend owner, I architected and implemented all client-facing features:

- **Real-Time Monitoring Dashboard:** A centralized view displaying live temperature, pressure, and speed data streamed via WebSockets.
- **Remote Reactor Control:** Secure interface for engineers to remotely start, stop, and adjust reactor setpoints, with immediate visual feedback.
- **Historical Data Analysis:** An interactive module allowing users to query, visualize, and export sensor data from past experiment runs.
- **Robust State Management:** Utilized Zustand to manage critical global state like authentication, API connectivity, and system-wide emergency stop status.
- **Secure Authentication:** Implemented a secure login flow using Google OAuth 2.0 and JWT.

---

## 🧠 Technical Challenges & Solutions

This project presented unique challenges, particularly in a tech-lead-absent environment.

### Challenge 1: Product Design & UX without a PM/Designer

- **Problem:** Without a Product Manager or UI/UX Designer, translating complex engineering requirements into an intuitive application was my direct responsibility.
- **Solution:** I established and led a direct, iterative feedback loop with the end-user chemical engineers. I created rapid prototypes in Figma [或者可以直接说: I rapidly prototyped UI concepts], conducted weekly demos, and used their feedback to refine the interface. This ensured the final product was not just technically functional but also user-centric and aligned with their critical workflows.

### Challenge 2: High-Frequency Data Rendering Performance

- **Problem:** The UI needed to render simultaneous, high-frequency data streams from multiple reactors without becoming sluggish or unresponsive, which would be unacceptable for a control system.
- **Solution:** I engineered a non-blocking UI by leveraging **React 18's `startTransition` API**. This powerful feature allowed me to mark state updates from WebSockets as "non-urgent," enabling React to prioritize user interactions (like typing in an input) over background data rendering. The result was a perfectly fluid UI, even under heavy data load.

#### Code Snippet: Non-Blocking State Updates

Here is a simplified, conceptual example of how `startTransition` was used to handle incoming WebSocket data:

```javascript
// This conceptual snippet demonstrates the non-blocking update pattern.
// It is not the original proprietary code.

import { startTransition } from 'react';

// Function to process messages from a WebSocket stream
function onWebSocketMessage(data) {

  // By wrapping the state update in startTransition, we tell React
  // that this is a non-urgent update.
  startTransition(() => {
    setControllers(prevControllers =>
      prevControllers.map(controller =>
        controller.id === data.id
          ? { ...controller, ...data.newValues }
          // Immutably update the state for the specific controller
          : controller
      )
    );
  });
}
