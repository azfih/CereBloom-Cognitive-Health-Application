# CereBloom: A Multimodal AI Framework for Cognitive Screening, Monitoring and Tele-Consultation

**Final Year Project, BS Computer Science, COMSATS University Islamabad (2022-2026)**
Authors: Hifza Rafiq, Hafsah Tariq | Supervisor: Ms. Qanetah Ahmed

**Keywords:** cognitive decline, Mini-Mental State Examination (MMSE), AI in healthcare, speech processing, medical image analysis, telemedicine, mobile health

---

## Abstract

Early detection of cognitive decline depends on structured history-taking, standardised cognitive testing and, where indicated, neuroimaging. These steps are time-consuming for clinicians and difficult to repeat over time. CereBloom is a mobile health system that automates the first-stage screening pathway and supports continued monitoring. It combines (i) verified history-taking forms, (ii) an AI-driven MMSE that accepts text, speech, image and drawing responses, (iii) optional MRI/CT analysis using a convolutional neural network, and (iv) a report generator that aggregates these sources into domain-level severity scores (memory, attention, language, executive function) with targeted recommendations. The system also provides gamified cognitive exercises, a daily wellness tracker, and a tele-consultation layer (appointments, payments, chat, audio and video calls) that connects patients with verified specialists.

## Demonstration

Demonstration videos covering the patient workflow (history-taking, AI-driven MMSE, imaging analysis), cognitive report generation and recommendations, cognitive games, the daily wellness tracker, and the telehealth module (appointments, payments and consultations) are available at the following link:

[CereBloom demonstration videos](https://drive.google.com/drive/folders/10QAv61ZA6zJm-KER4zsf4zowZIc8BZpA)
## Background and Objectives

Standard screening instruments such as the MMSE are administered manually, and follow-up depends on in-person visits. Existing consumer applications tend to focus on casual brain training, lack clinically standardised assessment, or do not give clinicians a means to follow a patient's progress remotely. Recent work on fully automated MMSE assessment using computer algorithms [1] and on the cognitive effects of structured brain-training games [2] motivates a system that brings these elements together.

The objectives of CereBloom were to:

1. Digitise a standardised screening workflow (history-taking and MMSE) with multimodal input.
2. Integrate optional imaging analysis as supporting evidence rather than a stand-alone diagnosis.
3. Produce an interpretable, domain-level cognitive report with longitudinal trends.
4. Support adherence through gamified exercises and a wellness tracker.
5. Enable remote clinical follow-up through a secure tele-consultation workflow.

---

## System Overview

CereBloom follows a three-role model (patient, doctor, administrator) and a staged patient pathway: onboarding, assessment, reporting, intervention, and consultation. The sections below describe each stage and its implementation.

### 1. Role-Based Onboarding and Verification

Patients, doctors and administrators register through role-specific flows. Patient registration includes an informed-consent step. Doctor registration collects professional credentials, and the licence is verified through an automated procedure (Selenium) before the account is activated.

### 2. Patient History and Assessment Hub

Patients complete a set of verified history-taking forms covering memory changes, psychological factors and daily functioning. Questions can be read aloud (text-to-speech) and answered by voice (speech-to-text) to reduce the barrier for older users. Each form is weighted according to its clinical relevance, and subsequent sections remain locked until the form is complete.



### 3. AI-Driven MMSE

The MMSE is organised into five categories: orientation, registration, attention, recall, and language/visual tasks. Responses are collected as text, speech, images or drawings. Speech is transcribed with Whisper, language responses are processed with spaCy, and visual and drawing responses are analysed using OpenCV, NumPy and MediaPipe. Computationally heavy analysis is performed by a FastAPI service, and category-wise scores are stored per user in Firestore.



### 4. Optional MRI/CT Analysis

Patients may upload a brain MRI or CT image. A CNN-based classifier, served through the FastAPI backend, returns a predicted category with a confidence value and probability scores, which are stored with the user record and passed to the report generator as supporting information.



### 5. Cognitive Health Report and Recommendations

The report module aggregates history-form scores, MMSE category scores and, where available, imaging output. It reports an overall score with status and risk level, domain-level scores with severity from "Normal" to "Severe Decline", historical trends over successive evaluations, and recommendations specific to each domain. Reports can be shared with healthcare providers.



### 6. Gamified Cognitive Exercises and Wellness Tracking

Cognitive exercises (memory pair matching, spot the difference, attention hexagon, merge puzzle) are implemented with the Flame engine. Performance, activity times and streaks are synchronised to Firebase and displayed in progress dashboards. A daily wellness tracker records medication, meals, exercise and meditation, and a mood tracker records day-to-day affect. Weekly recommendations are generated from these data.



### 7. Tele-Consultation, Appointments and Payments

Patients book appointments with verified doctors, pay through Stripe, and consult by chat, audio or video. Doctors manage schedules, capacity and consultation preferences. Notifications (Firebase Cloud Messaging and local notifications) remind users of appointments and exercises.

---

## System Architecture

<!-- Insert architecture diagram here -->
![System architecture](/images/architectural-diagram.png)

The mobile client (Flutter, BLoC state management) communicates with Firebase for authentication, data storage, file storage and messaging. Media-based assessment tasks (audio, image, drawing, scan) are sent to a Python FastAPI service that hosts the speech, NLP, vision and CNN components. Payments are handled through the Stripe SDK.

## Technology Stack

| Layer | Technologies |
|---|---|
| Mobile application | Flutter, Dart, BLoC, fl_chart, Flame |
| Backend and AI services | Python, FastAPI, TensorFlow (CNN), Whisper, spaCy, OpenCV, MediaPipe, NumPy |
| Data and authentication | Firebase Authentication, Cloud Firestore, Realtime Database, Firebase Storage |
| Messaging and notifications | Firebase Cloud Messaging, flutter_local_notifications |
| Payments | Stripe |
| Verification automation | Selenium |

## My Contributions

I was responsible for the backend and AI components of the system:

- Designed and implemented the AI-driven MMSE pipeline, the telehealth module, the payment and appointment flows, role-based registration and onboarding, report generation, and the recommendation module.
- Integrated the FastAPI backend and speech processing (Whisper) with the Flutter application.
- Designed the Firestore data model, implemented BLoC state management, and integrated Stripe payments.

## Limitations and Ethical Considerations

- CereBloom is a first-stage screening tool and does not replace clinical diagnosis; findings require confirmation by a qualified clinician.
- Part of the assessment relies on self-reported history, which may introduce reporting bias.
- The system has not undergone clinical validation, and its outputs should not be interpreted as diagnostic accuracy.
- Health data and medical images are sensitive; secure storage, access control and informed consent are treated as design requirements.
- The application is intended for continuous assessment and chronic care, not urgent cognitive emergencies.

## Future Work

Planned directions include clinical validation of the scoring pipeline, improved model precision and calibration, explainability of AI outputs for clinicians, wearable-based monitoring, and multilingual support.

## References

[1] L. Chen et al., "A fully automated Mini-Mental State Examination assessment model using computer algorithms for cognitive screening," *Journal of Alzheimer's Disease*, vol. 97, pp. 1661-1672, 2024.

[2] A. Al-Thaqib et al., "Brain training games enhance cognitive function in healthy subjects," *Medical Science Monitor Basic Research*, vol. 24, pp. 63-69, 2018.

---

*All rights reserved. Contact: [LinkedIn](https://linkedin.com/in/hifzarafiq)*
