
# libras-sign-classifier

**A Libras sign coach: it recognizes the sign you made and tells you which of the 5 Libras parameters you got wrong, and how to fix it.**

> **Status: work in progress.** This repository is being built in public. Sections marked *(planned)* describe what is coming next, not what exists today.

## The idea

Most sign language projects answer one question: *which sign is this?* That is useful, but it does not help someone who is learning.

A Libras sign is built from five parameters: **handshape** (configuração de mão), **location** (ponto de articulação), **movement**, **palm orientation** and **non-manual expressions** (face and body). When a learner signs incorrectly, the error is usually in one of them, and handshape is the most common one for hearing learners.

This project goes one step further than recognition:

```
"You signed <SIGN>.
 ✔ handshape   ✔ orientation   ✔ facial expression
 ✘ location: your hand started at chest height; the reference starts near the chin
 ✘ movement: your path was shorter than the reference; extend the movement"
```

## Who it is for

- Students learning Libras as a second language, including teacher-training and speech-therapy students, for whom Libras is a required course in Brazil.
- Families of deaf children.
- Libras teachers, who can see which parameters a whole class gets wrong most often *(planned)*.

## How it works

```
video ──► MediaPipe (hands + body + face) ──► per-parameter features ──► compare with reference signers ──► feedback
```

| Parameter | What is measured | Source |
|---|---|---|
| Handshape | Angles of every finger joint | MediaPipe Hands |
| Location | Hand position relative to face and chest | MediaPipe Pose |
| Orientation | Direction the palm is facing | MediaPipe Hands |
| Movement | Hand trajectory, compared with the reference using dynamic time warping (DTW) | Hands + Pose |
| Non-manual expressions | Eyebrows, mouth and head | MediaPipe Face (blendshapes) |

Instead of needing thousands of videos per sign, each sign has **3 to 5 reference videos recorded by deaf signers or Libras teachers**. The learner's sign is compared with them, parameter by parameter. A classifier recognizes which sign was attempted; the comparison explains what went wrong.

## Honest evaluation

Many sign recognition projects test on the same people they trained on, which inflates accuracy. Here, results are always reported in two ways:

| Model | Signs | Same signers (accuracy) | **Unseen signers (accuracy)** |
|---|---|---|---|
| Random Forest | TBD | TBD | TBD |
| MLP | TBD | TBD | TBD |

The *unseen signers* column is the number that matters: it predicts how the system works for a new user.

## Roadmap

- [ ] Landmark extraction (hands, body and face) for 100+ videos
- [ ] Classifier for 10 signs, with both evaluation numbers in this README
- [ ] Handshape and location feedback for 5 signs
- [ ] Public demo on Hugging Face Spaces
- [ ] Live demo at an innovation fair in Fortaleza (December 2026)
- [ ] All 5 parameters for 20 signs *(first half of 2027)*
- [ ] Real-time web version tested with Libras students *(2027)*

## Repository structure *(planned)*

```
libras-sign-classifier/
├── data/              # not versioned (see Data and privacy)
├── references/        # reference signs: landmarks only, no video
├── notebooks/         # exploration and experiments
├── src/
│   ├── extract.py     # video → landmarks (hands, pose, face)
│   ├── parameters.py  # handshape, location, orientation, movement, expression features
│   ├── classify.py    # which sign was attempted
│   ├── feedback.py    # per-parameter comparison with references
│   └── app.py         # Gradio demo
├── requirements.txt
└── README.md
```

## Getting started *(planned)*

```bash
git clone https://github.com/pedroarthurg1/libras-sign-classifier.git
cd libras-sign-classifier
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python src/app.py
```

## Data and privacy

Videos of people signing are personal data under Brazil's LGPD. Raw videos are not stored in this repository; only extracted landmarks are used. Reference signs are recorded only with written consent.

## Built with the deaf community

What counts as a *correct* sign is defined by deaf signers and Libras teachers, not by the model. Regional variation is real: a sign may be signed differently in Ceará and in São Paulo, and the reference set will say which variant it uses.

## Limitations

- Isolated signs only, no full sentences.
- Feedback quality depends on the reference signers and on camera conditions (lighting, framing).
- A coach for practice, not a replacement for a Libras teacher.

## Team and credits

Built by a student team from Ceará, Brazil. Code and experiments by [Pedro Arthur](https://github.com/pedroarthurg1).

Landmark detection by [MediaPipe](https://developers.google.com/mediapipe).

## License

To be defined withe the team
