<div align="center">

![header](https://capsule-render.vercel.app/api?type=waving&color=gradient&height=230&section=header&text=Hyeongmin%20Cho&fontSize=72&fontAlignY=34&desc=HRI%20%C2%B7%20Social%20Robotics%20%C2%B7%20Embodied%20AI&descAlignY=56&descSize=20)

**Human–Robot Interaction researcher @ [SIRLab](https://sirl.handong.edu/), Handong Global University**

*Building robots that people are willing to open up to — and measuring whether they actually do.*

[![Email](https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:goodjj02@gmail.com)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/h._.m1225?igsh=MXdoZTNiNWNobzk0cg==)
[![Threads](https://img.shields.io/badge/Threads-000000?style=for-the-badge&logo=threads&logoColor=white)](https://www.threads.net/@h._.m1225)

</div>

---

## About

I design **social robots that form emotional bonds**, and I run **controlled human-subject experiments** to find out what actually drives that bond.

The claim I keep testing: **competence is not what makes a robot likeable.** A robot that guesses wrong and owns its mistake is rated warmer, closer, and more disclosure-inviting than a flawless one — while a robot that simply *cannot proceed* is rated worst by every participant. I call the design strategy **lovable flaws (하찮미)**, and I build the hardware, the dialogue stack, and the study protocol end to end.

Lately that work has moved off the cloud: Moti's brain now runs on a **Jetson AGX Orin beside the robot**, where Gemma 4 E4B hears a person's voice directly, tone included, and answers in Korean.

| | |
|---|---|
| 🎓 **Affiliation** | Handong Global University — School of AI, Computer and Electrical Engineering (AICE)<br>Combined BS–MS Program |
| 🧪 **Lab** | [SIRLab](https://sirl.handong.edu/) — Social and Interactive Robotics Lab |
| 👨‍🏫 **Advisor** | Prof. WonHyong Lee (이원형 교수님) |
| 🔭 **Research** | Human–Robot Interaction · Social Robotics · Embodied AI · On-Device LLMs |
| 🛠️ **I do** | Robot bring-up → on-device LLM inference → real-time dialogue → experiment design → statistics → paper |
| 🌟 **Vision** | 선한 로봇 기술로 세상을 따뜻하게 바꾸는 로봇 공학자 |

---

## 📄 Publications

<!-- After the professor approves the ISR submission, delete this line and the closing line below to show the entry.
**[3]** **Hyeongmin Cho** et al. *Title withheld during blind review.*
**Intelligent Service Robotics** (Springer).
![Under Review](https://img.shields.io/badge/Status-Under%20Review-F9A825?style=flat-square)

-->
**[2]** **Hyeongmin Cho**, Eunseong Kang, WonHyong Lee.
*Separable Effects of Robot Error Type on Likeability and Perceived Intelligence: A Within-Subject Comparison.*
**RiTA 2026** — 14th International Conference on Robot Intelligence Technology and Applications, Daejeon, Korea (Springer LNNS).
![Accepted](https://img.shields.io/badge/Status-Accepted-2E7D32?style=flat-square) &nbsp;To be presented in December 2026.

**[1]** **조형민**, 강은성, 임혜원, 윤이름, 이원형.
*소셜 로봇의 의도적 비완전성(하찮미)이 사용자의 자기 개방 의도에 미치는 영향: 친밀감의 매개 효과를 중심으로*
(*The Effect of Social Robots' Intentional Imperfection (Lovable Flaws) on User's Self-Disclosure Intention: Focusing on the Mediating Role of Intimacy*).
**제21회 한국로봇종합학술대회 (KRoC 2026)**, 한국로봇학회 — 학부생 부문.
![Published](https://img.shields.io/badge/Status-Published-2E7D32?style=flat-square)

---

## 🤖 Moti — Empathy Service Robot

> *A social robot that earns intimacy by being imperfect on purpose.*

The platform behind both publications above: a costumed desktop-scale robot combining **display-based expression**, **physical gesture**, and **real-time voice dialogue**, across three generations.

https://github.com/user-attachments/assets/6e6d6663-b9ec-43d4-9311-3b2ba13880fd

<sub>**Left** — the costumed form all study participants met. &nbsp;**Right** — the bare mechanism: Dynamixel joints and wheels, display face, pan/tilt head.</sub>

### Two Jetsons: body and brain

Moti now runs on two boards. The body senses and acts. The brain listens and decides what to say, with no cloud in the loop. They talk over one WebSocket on the same LAN.

| | Board | What it does | Repo |
|---|---|---|---|
| 🦾 **Body** | Jetson Orin Nano Super 8GB | Microphone, echo cancellation, speaker · camera and face recognition · 11 Dynamixel motors · expression UI · runs every tool call | [HGU-SIRLab/MOTI-HRI](https://github.com/HGU-SIRLab/MOTI-HRI) |
| 🧠 **Brain** | Jetson AGX Orin 64GB | Silero VAD → smart-turn-v3 → Gemma 4 E4B → Piper TTS, served by vLLM | [HGU-SIRLab/MOTI_BRAIN](https://github.com/HGU-SIRLab/MOTI_BRAIN) |

**Brain highlights**

- 🔌 **One-line swap from Gemini Live** — a shim mimics the `google.genai` live session, so moving the robot to the local brain changed a single `connect()` call.
- 👂 **No speech-to-text** — Gemma 4 E4B takes audio directly and hears tone as well as words, which removed the STT and emotion-recognition models from the design.
- ⏱️ **Turn detection that respects thinking pauses** — Silero VAD plus smart-turn-v3, because natural pauses reached 2.98 s and silence alone kept cutting people off.
- 📏 **Measured on the real robot** — barge-in **0.10 s**, end of speech to first audio **median about 2.0 s**, first turn 17.9 s → **0.2 s** after warm-up, 13.2 tok/s decode.
- ⚖️ **The trade-off, stated plainly** — Gemini Live answers in about 0.5 s and has an expressive voice. The local brain gives up both in exchange for no API cost or rate limits, voices that stay on our own hardware, and a robot whose personality doesn't change when a provider updates its model.

**Body highlights**

- 🗣️ **Real-time voice dialogue with true barge-in** — cut the robot off mid-sentence and it stops.
- 🧠 **Curiosity persona over slot-filling** — the scripted 7-stage interview FSM is gone; facts accumulate implicitly through **function calling** (`remember_fact`, `set_emotion`) during natural conversation, and `forget_me` deletes the profile and the face together.
- 🦾 **3-layer motion safety** — hardcoded macros (greeting / hug / shy / dance) → parameterised primitives where the LLM picks only *intensity · speed · repeat* and code maps them into safe joint ranges → no-op fallback. 11 Dynamixel motors (9 joints + 2 wheels) over U2D2.
- 👁️ **On-robot perception** — InsightFace embeddings with FuzzyART online learning to recognize people, and FaceLandmarker with PID for pan/tilt tracking.
- 🎮 **Study-grade task engine** — a zoomed-in object quiz with three scripted personas (correct / wrong-but-engaged / unable-to-answer), so conditions stay reproducible across 30 participants.

**Edge port — Jetson Orin Nano Super.** Moving v3 off its laptop tether onto a fully on-board Orin Nano (JetPack 6.2.3, MAXN_SUPER, NVMe): resolved a three-way `numpy`/`opencv-contrib`/`onnx` conflict, confirmed the ARM MediaPipe dotprod crash doesn't hit Cortex-A78AE, and fixed the genuinely hard part — embedded audio, where the robot's own 48 kHz speaker output kept false-triggering barge-in until it was routed through **PulseAudio `module-echo-cancel` AEC**. Bring-up is automated by a `jetson_doctor.py` self-check.

### Three generations

| | Focus | Repo |
|---|---|---|
| **mk1** | Intentional imperfection, multi-user entertainment | [HandongSF/hlri-iua-motirobotics](https://github.com/HandongSF/hlri-iua-motirobotics) |
| **mk2** | 1:1 conversation, layered architecture (core / hardware / vision / display) | [HGU-SIRLab/Empathy-service-motirobot](https://github.com/HGU-SIRLab/Empathy-service-motirobot) |
| **v3** | Real-time voice dialogue and curiosity persona, first on Gemini Live, now on the local brain | [HGU-SIRLab/MOTI-HRI](https://github.com/HGU-SIRLab/MOTI-HRI) + [HGU-SIRLab/MOTI_BRAIN](https://github.com/HGU-SIRLab/MOTI_BRAIN) |

---

## 🧠 HARU — Embodied Social AI &nbsp;<sub>development ended</sub>

[![Repository](https://img.shields.io/badge/HGU--SIRLab%2FHARU--embodied--social--ai-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/HGU-SIRLab/HARU-embodied-social-ai)
![Development Ended](https://img.shields.io/badge/Status-Development%20Ended-6E7681?style=for-the-badge)

> *Embodied Social AI for Proactive Human–Robot Interaction.*

https://github.com/user-attachments/assets/ac65ecc8-21c3-478a-b1b2-c713c3e09ad8

Most social robots wait to be spoken to. HARU watched — facial expression, posture, vocal tone — and **decided on its own when to start a conversation**. The entire reasoning stack ran locally on the robot; nothing left the device.

**Development has ended and the repo stays up as a record.** What follows is the architecture as built and benchmarked. The Jetson AGX Orin and the vLLM serving work carried over to Moti's brain.

**Triple-system architecture**, after Kahneman's dual-process theory:

| Layer | Role | Implementation |
|---|---|---|
| **System 3** | Always-on perception | CPU Haar-cascade attention, 5-state FSM (`EMPTY → APPEARED → CONVERSING`) |
| **System 2** | Event-driven reasoning | Gemma 4 12B multimodal, 6-stage Theory-of-Mind pipeline |
| **System 1** | Continuous action | 50 Hz control loop, 9 position joints + 2 wheel drives |

**Making a 12B VLM conversational on an edge board.** Naïve HuggingFace bf16 took ~45 s per turn — unusable for dialogue. The final result was **~7.4 s for full inference**, with first speech as low as **0.65 s on a warm cache (69× faster)** and 2–3 s typical once the Social World Model had history:

- **W4A16 quantization** via AutoRound, with custom RoPE-dimension patching and a Jetson-specific fix for a PyTorch 2.5 `set_submodule` bug that silently blocked quantized-layer installation
- **vLLM + Marlin INT4 kernels + CUDAGraph** → ~18 tok/s decode, with prefix caching across turns
- A **TRT-LLM** compiled-engine path (8–16 h build) and an HF bf16 baseline kept for comparison; `llama.cpp` evaluated and dropped
- Latency also bought back in the boring places: streaming callbacks, image 448→336, abbreviated JSON action keys (−32 tokens/turn)

**Memory and learning** — pipelines built; validation with people was still ahead when development ended

- **Social World Model** — what the robot knows about a person, persisted across sessions
- **Episodic LoRA adapters** so new people don't overwrite old ones (catastrophic forgetting)
- **Kinesthetic teaching** — torque off the joints, move the robot by hand, and it records the motion
- **Human-in-the-loop correction** (accept / direct / manual / skip) feeding the fine-tuning set

**Platform** — NVIDIA Jetson AGX Orin 64 GB · JetPack 6.2.2 (CUDA 12.6) · ROS 2 Humble · Intel RealSense SR300 + Logitech C270 · 11 Dynamixel XM/XH servos over U2D2

---

## 🦸 Herobot — Physical AI Agent &nbsp;<sub>archived</sub>

[![Repository](https://img.shields.io/badge/Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/ChoHyeongmin1225/Herobotics)
![Discontinued](https://img.shields.io/badge/Status-Discontinued-6E7681?style=for-the-badge)

> *From language to action: a rescue robot with a value system.*

https://github.com/user-attachments/assets/2e13df92-c2dc-4fd8-ae6c-1fa370db7e67

An attempt to wire **LLM reasoning directly to physical actuation** — natural-language intent becoming control signals, filtered through an explicit heroic persona so the robot would decide *whether* to act, not just *how*. Built on Gemini Robotics.

**It didn't work out, and I stopped.** The repo stays up as a record. What survived is the idea it forced me to work out: if a language model is allowed to command joints, the safety has to live in the layers *below* it, not in the prompt. That layered action-safety pattern is exactly what Moti v3's motion stack was later built on — so the failure paid for itself.

---

## 🔭 What's Next

Moti's local brain now carries a full conversation on the physical robot. The question left is the research one: **is a local model good enough for empathetic conversation in Korean?** That quality baseline is what I measure next, along with the two gaps left against Gemini Live: response latency and emotion in the voice.

---

## 🛠️ Tech Stack

<div align="center">

**Robotics**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![ROS2](https://img.shields.io/badge/ROS_2_Humble-22314E?style=for-the-badge&logo=ros&logoColor=white)
![Jetson](https://img.shields.io/badge/Jetson_AGX_Orin_·_Orin_Nano_Super-76B900?style=for-the-badge&logo=nvidia&logoColor=white)
![CUDA](https://img.shields.io/badge/CUDA-76B900?style=for-the-badge&logo=nvidia&logoColor=white)
![Dynamixel](https://img.shields.io/badge/Dynamixel_SDK-FF6F00?style=for-the-badge&logo=robotframework&logoColor=white)
![RealSense](https://img.shields.io/badge/RealSense-0071C5?style=for-the-badge&logo=intel&logoColor=white)

**On-Device LLM & Inference**

![Gemma](https://img.shields.io/badge/Gemma-4285F4?style=for-the-badge&logo=google&logoColor=white)
![Qwen](https://img.shields.io/badge/Qwen-615CED?style=for-the-badge&logo=alibabacloud&logoColor=white)
![vLLM](https://img.shields.io/badge/vLLM-FDB515?style=for-the-badge&logo=v&logoColor=black)
![TensorRT-LLM](https://img.shields.io/badge/TensorRT--LLM-76B900?style=for-the-badge&logo=nvidia&logoColor=white)
![Quantization](https://img.shields.io/badge/W4A16_·_AutoRound_·_Marlin_INT4-8E44AD?style=for-the-badge&logo=lightning&logoColor=white)
![LoRA](https://img.shields.io/badge/LoRA_/_PEFT-FF9E0F?style=for-the-badge&logo=huggingface&logoColor=white)

**Voice Pipeline**

![Silero VAD](https://img.shields.io/badge/Silero_VAD-2E7D32?style=for-the-badge)
![smart-turn-v3](https://img.shields.io/badge/smart--turn--v3-455A64?style=for-the-badge)
![Piper TTS](https://img.shields.io/badge/Piper_TTS-00838F?style=for-the-badge)
![WebSocket](https://img.shields.io/badge/WebSocket-010101?style=for-the-badge)

**Perception & Learning**

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Transformers](https://img.shields.io/badge/Transformers-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![ONNX Runtime](https://img.shields.io/badge/ONNX_Runtime-005CED?style=for-the-badge&logo=onnx&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-00A8E1?style=for-the-badge&logo=google&logoColor=white)
![InsightFace](https://img.shields.io/badge/InsightFace-5D4037?style=for-the-badge)

**Cloud LLM APIs**

![Gemini Live API](https://img.shields.io/badge/Gemini_Live_API-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white)
![Gemini Robotics](https://img.shields.io/badge/Gemini_Robotics-1A73E8?style=for-the-badge&logo=google&logoColor=white)

**Research & Analysis**

![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=for-the-badge&logo=mathworks&logoColor=white)
![LaTeX](https://img.shields.io/badge/LaTeX-008080?style=for-the-badge&logo=latex&logoColor=white)

**Systems**

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

</div>

---

<div align="center">

<br><br>

### 📬 Get in touch

**Always happy to talk about social robots, HRI study design, or squeezing a VLM onto an edge board.**

[![Email](https://img.shields.io/badge/Gmail-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:goodjj02@gmail.com)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/h._.m1225?igsh=MXdoZTNiNWNobzk0cg==)
[![Threads](https://img.shields.io/badge/Threads-000000?style=for-the-badge&logo=threads&logoColor=white)](https://www.threads.net/@h._.m1225)

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&height=120&section=footer" />

</div>
