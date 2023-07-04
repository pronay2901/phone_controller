// phone_controller
from flask import Flask, request

app = Flask(__name__)

connected_phones = {}  # Stores the connected phones and their information


@app.route('/connect', methods=['POST'])
def connect():
    phone_id = request.form.get('phone_id')
    connected_phones[phone_id] = {
        'phone_info': request.form.get('phone_info'),
        'command': ''
    }
    return 'Connected'


@app.route('/send_command', methods=['POST'])
def send_command():
    phone_id = request.form.get('phone_id')
    command = request.form.get('command')
    connected_phones[phone_id]['command'] = command
    return 'Command sent'


@app.route('/get_command', methods=['GET'])
def get_command():
    phone_id = request.args.get('phone_id')
    if phone_id in connected_phones:
        command = connected_phones[phone_id]['command']
        connected_phones[phone_id]['command'] = ''
        return command
    else:
        return 'Phone not connected'


if __name__ == '__main__':
    app.run()
