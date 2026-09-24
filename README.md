# Afya Smart ML

A clinician uploads an eConsult note. The app checks it against medical-record documentation guidelines, drafts the questions a specialist still needs, and proposes next steps the clinician rates before anything moves forward.

Built for the AfyaChat workflow used by primary care providers (PCPs) and specialists. Short referral notes often omit history, medications, allergies, and exam findings. This prototype surfaces those gaps before the consult is sent.

## What a user does

1. Sign in as a PCP or a specialist (SQLite).
2. Open a reference case (bone fracture, oral surgery, pneumonia) or upload an eConsult text file.
3. **Missing information.** The note is compared with Community First Health Plans documentation guidelines. The PCP gets a checklist, can add notes, and receives the list by SMS (Twilio).
4. **Targeted questions.** Three to four specialist-facing questions are drafted from the same note and guidelines.
5. **Recommendations.** Suggestions cover diagnostic testing, medication, and patient education. The clinician scores them on clinical soundness, likelihood of harm, relevance, completeness, and helpfulness.

The operations screen unlocks each step only after the previous one is done.

## How it works

Guideline text and the consult note are sent together to GPT-3.5 Turbo. Nothing is fine-tuned. The model is asked for a bounded output: the top missing items, a short question list, or three recommendation types. The clinician edits and rates the result; the model does not file the consult.

`app/starter.py` is a separate LlamaIndex prototype. It indexes a local document folder and answers a clinical query. It is not called from the Flask routes yet.

## Where to read the code

| Question | File |
| --- | --- |
| Login, upload, and the three clinical routes | `app/routes.py` |
| Missing-information prompt and checklist parsing | `app/userstory1.py` |
| Specialist question prompt | `app/userstory2.py` |
| Recommendation prompt | `app/userstory3.py` |
| Clinician rating form | `app/templates/recommendations.html` |
| Retrieval prototype | `app/starter.py` |
| Sample consults | `app/data_econsult/` |

## Run

```sh
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python run.py
```

Open [http://127.0.0.1:4444](http://127.0.0.1:4444).

Put `OPENAI_API_KEY`, and the Twilio `ACCOUNT_SID`, `AUTH_TOKEN`, `TWILIO_NUMBER`, and `TARGET_NUMBER`, in `config.py` on your machine. Do not commit that file.

Stack: Python, Flask, OpenAI, SQLite, Twilio, LlamaIndex, pytest, Selenium.

## Scope

This is a decision-support prototype. It does not diagnose, prescribe, or replace specialist review. Sample notes are for development only.
