import requests
import json

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    headers = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    input_json = { "raw_document": { "text": text_to_analyze } }
    
    response = requests.post(url, headers=headers, json=input_json)
    
    # Convert response text to dictionary
    response_dict = json.loads(response.text)
    
    # Extract emotion predictions
    emotion_predictions = response_dict.get('emotionPredictions', [])
    
    if emotion_predictions:
        # Get the emotion scores from the first prediction
        emotion_data = emotion_predictions[0].get('emotion', {})
        
        # Extract individual emotion scores
        anger_score = emotion_data.get('anger', 0)
        disgust_score = emotion_data.get('disgust', 0)
        fear_score = emotion_data.get('fear', 0)
        joy_score = emotion_data.get('joy', 0)
        sadness_score = emotion_data.get('sadness', 0)
        
        # Create dictionary with all emotion scores
        emotions = {
            'anger': anger_score,
            'disgust': disgust_score,
            'fear': fear_score,
            'joy': joy_score,
            'sadness': sadness_score
        }
        
        # Find the dominant emotion (highest score)
        dominant_emotion = max(emotions, key=emotions.get)
        
        # Return the formatted output
        return {
            'anger': anger_score,
            'disgust': disgust_score,
            'fear': fear_score,
            'joy': joy_score,
            'sadness': sadness_score,
            'dominant_emotion': dominant_emotion
        }
    else:
        # Return default values if no emotion predictions
        return {
            'anger': 0,
            'disgust': 0,
            'fear': 0,
            'joy': 0,
            'sadness': 0,
            'dominant_emotion': None
        }