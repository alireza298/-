from flask import Flask, request, jsonify
from flask_cors import CORS
import requests

app = Flask(__name__)
CORS(app)

AI_API = "http://5.161.91.18/chat"

@app.route("/analyze", methods=["POST"])
def analyze():
    data = request.json
    question = data.get("question", "")

    if not question:
        return jsonify({"error": "سوال خالیه، ذهن‌خوان نیستم"}), 400

    try:
        r = requests.get(AI_API, params={"text": question}, timeout=15)
        return jsonify(r.json())
    except Exception as e:
        return jsonify({"error": "AI حال نداشت جواب بده"}), 500


if __name__ == "__main__":
    app.run(debug=True)
    
