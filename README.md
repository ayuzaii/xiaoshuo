# xiaoshuo
from flask import Flask, request, jsonify, render_template
import tensorflow as tf
from generate_text import generate_text

app = Flask(__name__)
model, id_to_word = tf.keras.models.load_model('./model.h5'), pickle.load(open('id_to_word.pkl', 'rb'))

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        topic = request.form['topic']
        length = request.form['length']
        text = generate_text(model, id_to_word, topic, length)
        return jsonify({'text': text})
    else:
        return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
