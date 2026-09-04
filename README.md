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

Lately that work has moved on-device: getting a 12B vision-language model to reason about a person's face and tone **locally, on the robot**, fast enough that the conversation still feels alive.

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

**[2]** **Hyeongmin Cho**, Eunseong Kang, WonHyong Lee.
*Separable Effects of Robot Error Type on Likeability and Perceived Intelligence: A Within-Subject Comparison.*
**RiTA 2026** — 14th International Conference on Robot Intelligence Technology and Applications, Daejeon, Korea (Springer LNNS).
![Under Review](https://img.shields.io/badge/Status-Under%20Review-F9A825?style=flat-square)

**[1]** **조형민**, 강은성, 임혜원, 윤이름, 이원형.
*소셜 로봇의 의도적 비완전성(하찮미)이 사용자의 자기 개방 의도에 미치는 영향: 친밀감의 매개 효과를 중심으로*
(*The Effect of Social Robots' Intentional Imperfection (Lovable Flaws) on User's Self-Disclosure Intention: Focusing on the Mediating Role of Intimacy*).
**제21회 한국로봇종합학술대회 (KRoC 2026)**, 한국로봇학회 — 학부생 부문.
![Published](https://img.shields.io/badge/Status-Published-2E7D32?style=flat-square)

---

## 🧠 HARU — Embodied Social AI

[![Repository](https://img.shields.io/badge/HGU--SIRLab%2FHARU--embodied--social--ai-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/HGU-SIRLab/HARU-embodied-social-ai)
![In Development](https://img.shields.io/badge/Status-In%20Development-F9A825?style=for-the-badge)

> *Embodied Social AI for Proactive Human–Robot Interaction.*

https://github.com/user-attachments/assets/ac65ecc8-21c3-478a-b1b2-c713c3e09ad8

Most social robots wait to be spoken to. HARU watches — facial expression, posture, vocal tone — and **decides on its own when to start a conversation**. The entire reasoning stack runs locally on the robot; nothing leaves the device.

**This is active work, not a finished system** — what follows is the architecture as built and benchmarked so far.

**Triple-system architecture**, after Kahneman's dual-process theory:

| Layer | Role | Implementation |
|---|---|---|
| **System 3** | Always-on perception | CPU Haar-cascade attention, 5-state FSM (`EMPTY → APPEARED → CONVERSING`) |
| **System 2** | Event-driven reasoning | Gemma 4 12B multimodal, 6-stage Theory-of-Mind pipeline |
| **System 1** | Continuous action | 50 Hz control loop, 9 position joints + 2 wheel drives |

**Making a 12B VLM conversational on an edge board.** Naïve HuggingFace bf16 took ~45 s per turn — unusable for dialogue. Current best is **~7.4 s end-to-end (≥69× faster)**, and still coming down:

- **W4A16 quantization** via AutoRound, with custom RoPE-dimension patching and a Jetson-specific fix for a PyTorch 2.5 `set_submodule` bug that silently blocked quantized-layer installation
- **vLLM + Marlin INT4 kernels + CUDAGraph** → ~18 tok/s decode, with prefix caching across turns
- A **TRT-LLM** compiled-engine path (8–16 h build) and an HF bf16 baseline kept for comparison; `llama.cpp` evaluated and dropped
- Latency also bought back in the boring places: streaming callbacks, image 448→336, abbreviated JSON action keys (−32 tokens/turn)

**Memory and learning** — the part I'm building out now

- **Social World Model** — what the robot knows about a person, persisted across sessions
- **Episodic LoRA adapters** so new people don't overwrite old ones (catastrophic forgetting)
- **Kinesthetic teaching** — torque off the joints, move the robot by hand, and it records the motion
- **Human-in-the-loop correction** (accept / direct / manual / skip) feeding the fine-tuning set

**Platform** — NVIDIA Jetson AGX Orin 64 GB · JetPack 6.2.2 (CUDA 12.6) · ROS 2 Humble · Intel RealSense SR300 + Logitech C270 · 11 Dynamixel XM/XH servos over U2D2

---

## 🤖 Moti — Empathy Service Robot

> *A social robot that earns intimacy by being imperfect on purpose.*

The platform behind both publications above: a costumed desktop-scale robot combining **display-based expression**, **physical gesture**, and **real-time voice dialogue**, across three generations.

https://github.com/user-attachments/assets/6e6d6663-b9ec-43d4-9311-3b2ba13880fd

<sub>**Left** — the costumed form all study participants met. &nbsp;**Right** — the bare mechanism: 10 Dynamixel joints, display face, pan/tilt head.</sub>

| | Focus | Repo |
|---|---|---|
| **mk1** | Intentional imperfection, multi-user entertainment | [HandongSF/hlri-iua-motirobotics](https://github.com/HandongSF/hlri-iua-motirobotics) |
| **mk2** | 1:1 conversation, layered architecture (core / hardware / vision / display) | [HGU-SIRLab/Empathy-service-motirobot](https://github.com/HGU-SIRLab/Empathy-service-motirobot) |
| **v3** | Real-time voice dialogue, curiosity persona, on-robot GPU perception | [HGU-SIRLab/MOTI-HRI](https://github.com/HGU-SIRLab/MOTI-HRI) |

**v3 highlights**

- 🗣️ **Real-time voice dialogue** — Gemini Live API over WebSocket with server-side VAD and **true barge-in**: cut the robot off mid-sentence and it stops. Replaced the hand-rolled mouth-shape VAD of earlier generations.
- 🧠 **Curiosity persona over slot-filling** — the scripted 7-stage interview FSM is gone; facts accumulate implicitly through **function calling** (`remember_fact`, `set_emotion`) during natural conversation.
- 🦾 **3-layer motion safety** — hardcoded macros (greeting / hug / shy / dance) → parameterised primitives where the LLM picks only *intensity · speed · repeat* and code maps them into safe joint ranges → no-op fallback. 10 Dynamixel joints over U2D2.
- 👁️ **On-robot GPU perception** — face detection at **75 ms/frame on CUDA** (onnxruntime-gpu), plus MediaPipe gesture recognition driving pan/tilt tracking.
- 🎮 **Study-grade task engine** — a zoomed-in object quiz with three scripted personas (correct / wrong-but-engaged / unable-to-answer), so conditions stay reproducible across 30 participants.

**Edge port — Jetson Orin Nano Super.** Moving v3 off its laptop tether onto a fully on-board Orin Nano (JetPack 6.2.3, MAXN_SUPER, NVMe): resolved a three-way `numpy`/`opencv-contrib`/`onnx` conflict, confirmed the ARM MediaPipe dotprod crash doesn't hit Cortex-A78AE, and fixed the genuinely hard part — embedded audio, where the robot's own 48 kHz speaker output kept false-triggering barge-in until it was routed through **PulseAudio `module-echo-cancel` AEC**. Bring-up is automated by a `jetson_doctor.py` self-check.

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

Moti's dialogue still depends on a cloud endpoint. I'm building a **fully local replacement for the Gemini Live API** — on-device speech, reasoning, and voice with the barge-in latency of a hosted realtime API, using the quantized-inference stack from HARU. Repo to come.

---

## 🛠️ Tech Stack

<div align="center">

**Robotics**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![ROS2](https://img.shields.io/badge/ROS_2_Humble-22314E?style=for-the-badge&logo=ros&logoColor=white)
![Jetson](https://img.shields.io/badge/Jetson_AGX%2FOrin-76B900?style=for-the-badge&logo=nvidia&logoColor=white)
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

**Perception & Learning**

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Transformers](https://img.shields.io/badge/Transformers-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![ONNX Runtime](https://img.shields.io/badge/ONNX_Runtime-005CED?style=for-the-badge&logo=onnx&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-00A8E1?style=for-the-badge&logo=google&logoColor=white)

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

<img src="https://github-readme-stats-sigma-five.vercel.app/api?username=ChoHyeongmin1225&show_icons=true&theme=transparent" alt="GitHub Stats" />

<br><br>

### 📬 Get in touch

**Always happy to talk about social robots, HRI study design, or squeezing a VLM onto an edge board.**

[![Email](https://img.shields.io/badge/Gmail-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:goodjj02@gmail.com)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/h._.m1225?igsh=MXdoZTNiNWNobzk0cg==)
[![Threads](https://img.shields.io/badge/Threads-000000?style=for-the-badge&logo=threads&logoColor=white)](https://www.threads.net/@h._.m1225)

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&height=120&section=footer" />

</div>
