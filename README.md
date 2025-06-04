# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

# Date:
# Register no:212222230155
# Aim: 
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools

# System Architecture Components:
1.API Interaction – Simulated patient note fetched via static text.

2.AI Tools Integration:

  Hugging Face Transformers (real sentiment analysis),
  OpenAI GPT (real summarization),
  Claude, Gemini, Meta (simulated summarization)
  
3.Comparison and Analysis – Side-by-side comparison of AI responses.

4.Actionable Insight Generation – Extracted from AI outputs.

 # AI Tool Prompts:
 | Tool        | Prompt                                                                                 |
| ----------- | -------------------------------------------------------------------------------------- |
| **ChatGPT** | "Summarize the following clinical note and identify key symptoms: {text}"              |
| **Claude**  | "Read the clinical note and provide potential health risks and treatment suggestions." |
| **Gemini**  | "Analyze this patient report and list 3 recommended actions for physicians."           |
| **Meta**    | "Extract symptoms and possible urgent care needs from the patient history below."      |

# Python Code:
```
import openai
from transformers import pipeline

# Set up your OpenAI API key
openai.api_key = "your_openai_api_key_here"

# Simulated patient data (API input)
patient_note = """
Patient is a 62-year-old male with a history of hypertension and type 2 diabetes. 
Reports chest tightness and shortness of breath during mild exertion. 
BP recorded at 160/100 mmHg, HR 98 bpm. 
Complains of occasional dizziness and fatigue. 
Family history of heart disease.
"""

# Hugging Face sentiment analysis (acts as condition polarity check)
sentiment_analyzer = pipeline("sentiment-analysis")

def analyze_with_openai(note):
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are a helpful medical assistant."},
            {"role": "user", "content": f"Summarize the following clinical note and identify key symptoms:\n{note}"}
        ]
    )
    return response['choices'][0]['message']['content']

def simulate_claude(note):
    return "The patient may be at risk of cardiac issues. Recommend ECG, BP monitoring, and lifestyle changes."

def simulate_gemini(note):
    return "1. Schedule cardiologist appointment. 2. Monitor blood pressure daily. 3. Assess for possible angina."

def simulate_meta(note):
    return "Symptoms include chest tightness, dizziness, and fatigue. Potential urgent care need for cardiac evaluation."

def compare_outputs(outputs):
    print("\n=== COMPARATIVE OUTPUTS ===")
    for name, output in outputs.items():
        print(f"\n{name}:\n{output}")

def generate_insights(outputs):
    all_text = " ".join(outputs.values()).lower()
    insights = []
    if "cardiac" in all_text or "heart" in all_text:
        insights.append("⚠️ Recommend ECG and cardiology review.")
    if "bp" in all_text or "blood pressure" in all_text:
        insights.append("📈 Monitor BP regularly and review hypertensive medication.")
    if "dizziness" in all_text or "fatigue" in all_text:
        insights.append("🔄 Evaluate for neurological or metabolic causes of dizziness.")
    return insights
```
# Run Analysis
```
print("Running Healthcare Bot...\n")

openai_summary = analyze_with_openai(patient_note)
hf_sentiment = sentiment_analyzer(patient_note)[0]

# Simulated tools
claude_output = simulate_claude(patient_note)
gemini_output = simulate_gemini(patient_note)
meta_output = simulate_meta(patient_note)

outputs = {
    "OpenAI GPT-4": openai_summary,
    "HuggingFace Sentiment": f"{hf_sentiment['label']} ({hf_sentiment['score']:.2f})",
    "Claude": claude_output,
    "Gemini": gemini_output,
    "Meta": meta_output
}

compare_outputs(outputs)

print("\n=== ACTIONABLE INSIGHTS ===")
for insight in generate_insights(outputs):
    print(insight)
```

# Output:

| Tool              | Output Snippet                                                               |
| ----------------- | ---------------------------------------------------------------------------- |
| **ChatGPT (GPT)** | "Patient presents signs of cardiac stress... chest pain, high BP, fatigue."  |
| **Claude**        | "Risk of cardiac issue, recommend ECG and medication adjustment."            |
| **Gemini**        | "Schedule cardiology visit, monitor BP, assess for angina."                  |
| **Meta**          | "Urgent symptoms: chest pain, dizziness. Action: cardiac check recommended." |

# Comparison Summary:
| Tool       | Summary / Output                                               |
| ---------- | -------------------------------------------------------------- |
| **OpenAI** | Detailed and human-like medical summary of patient conditions. |
| **Claude** | Condensed medical risk-focused statement (simulated).          |
| **Gemini** | Direct medical recommendation format (simulated).              |
| **Meta**   | Broad clinical interpretation of symptoms (simulated).         |

# Conclusion:

This experiment successfully demonstrates the integration of multiple AI tools into a healthcare assistant bot. The modular Python code structure allows for easy plugin of various AI APIs, real-time analysis, and insight generation. Comparing multiple AI outputs can enhance clinical validation and ensure consistency across automated systems.


# Result:
The corresponding Prompt is executed successfully.
