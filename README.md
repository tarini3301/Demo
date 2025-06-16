# Demo
Great, Tarini! Since this is Day 1 of your DRDO DLRL offline GPT-like assistant project and open-source LLMs are already downloaded, here’s a structured roadmap for what to do today to get moving confidently:

⸻

✅ DAY 1 PLAN – “BOOTSTRAP YOUR OFFLINE GPT SYSTEM”

⸻

🔍 Step 1: Understand What’s Available

First, check:
	•	✅ What models are downloaded (e.g., mistral, llama2, gemma, code-llama)
	•	✅ What format they are in:
	•	.gguf → used with llama.cpp / Ollama
	•	.safetensors → used with Transformers / Text Generation WebUI
	•	✅ Where they are stored (path)

Command to list:

ls ~/models/  # or wherever DRDO team stored them


⸻

🧠 Step 2: Choose an LLM Backend

Based on your team’s setup, pick one:

Backend Tool	Use When	Command to Check
Ollama	For easy setup	ollama list
llama.cpp	For fast + light CLI	./main -m model.gguf
text-generation-webui	For GUI + advanced tuning	./start_linux.sh

✔️ Recommended: Use Ollama if available (easiest API access).

⸻

⚙️ Step 3: Run the LLM Locally

If using Ollama:

ollama run mistral

Try a test call from terminal:

curl http://localhost:11434/api/generate -d '{"model": "mistral", "prompt": "Explain DRDO."}'

✔️ If this gives output, you’re ready to connect it to a web server.

⸻

🌐 Step 4: Set Up a Basic Web Server (Flask)

app.py — Flask server

from flask import Flask, request, jsonify
import requests

app = Flask(__name__)

@app.route('/chat', methods=['POST'])
def chat():
    user_prompt = request.json['prompt']
    res = requests.post("http://localhost:11434/api/generate", json={
        "model": "mistral",
        "prompt": user_prompt
    })
    return jsonify(res.json())

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

pip install flask requests
python app.py


⸻

💻 Step 5: Test via Browser or curl

From any LAN-connected device:

curl -X POST http://<your-ip>:5000/chat -H "Content-Type: application/json" -d '{"prompt": "What is radar?"}'

Replace <your-ip> with your server machine’s LAN IP.

⸻

🧪 Step 6: Optional: Minimal Frontend

Create a frontend.html and open in browser:

<input id="prompt" placeholder="Ask something...">
<button onclick="send()">Send</button>
<pre id="response"></pre>
<script>
async function send() {
  let prompt = document.getElementById('prompt').value;
  let res = await fetch('http://<your-ip>:5000/chat', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({prompt})
  });
  let data = await res.json();
  document.getElementById('response').innerText = data.response;
}
</script>


⸻

📝 Day 1 Goals Recap:

Task	Done?
✅ Identify model and backend (Ollama or llama.cpp)	🔲
✅ Run the model and test with sample prompt	🔲
✅ Set up basic Flask API to call model	🔲
✅ Test API call from browser / curl	🔲
✅ Optional: Simple frontend HTML	🔲


⸻

🎯 Your Focus for Day 1:

“Successfully run the downloaded model locally, expose it over LAN via Flask, and get a prompt working from any LAN device.”

⸻

Would you like me to give you a ZIP of starter files (Flask server + HTML frontend + usage instructions) to save time today?
