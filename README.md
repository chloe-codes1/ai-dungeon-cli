# AI Dungeon API

This is basically a cli client to [play.aidungeon.io](https://play.aidungeon.io/).

This allows using gpt-3!

# How to use it?
```python
from pprint import pprint

import requests

host = 'https://dev.gpt-3.whatilearened.today'

def get_scenes():
    scenes = requests.get(f"{host}/scenes").json()
    pprint(scenes)
    return scenes


def create_session(name: str, scene: str = 'qa'):
    data = {
        "name": name,
        "scene": scene,
    }
    resp = requests.post(f"{host}/sessions", json=data).json()
    pprint(resp)
    session_id = resp['id']

    def send_msg(msg):
        msg = requests.post(f"{host}/sessions/{session_id}/message", json={'message': msg}).json()
        print(msg['text'])
        return msg

    def show_history():
        results = requests.get(f"{host}/sessions/{session_id}").json()['history']
        for h in results:
            print('-' * 10)
            print(f"type: {h['history_by']}")
            print(f"{h['text']}")
            print('-' * 10)

    return send_msg, show_history


def add_scene(name, text):
    data = {
        "text": text,
        "name": name,
    }
    try:
        resp = requests.post(f"{host}/scenes", json=data).json()
        pprint(resp)

    except Exception as e:
        print(e)


def delete_scene(name):
    try:
        result = requests.delete(f"{host}/scenes/{name}")
        print(result.text)
    except Exception as e:
        print(e)
if __name__ == '__main__':

    get_scenes()
    name = 'test1'
    send_msg, show_history = create_session(name)

    q = 'Q: Who is Apple CEO?'
    print('\n\n', q)
    send_msg(q)

    q = 'Q: Who is MicroSoft CEO?'
    send_msg(q)

    print(f'\n\n{"#" * 6} print all history {"#" * 6}')
    pprint(show_history())

```
[show more samples](sample.py)