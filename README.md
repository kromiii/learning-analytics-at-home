# 自宅で始める Learning Analytics Platform の構築

https://zenn.dev/kromiii/books/bd5dd8ca2de0a4

## LMS (moodle) の起動

```
make start-moodle
```

http://localhost でアクセスできる

user と password は user/bitnami

## LRS (Learning Locker) の起動

```
make start-learninglocker
```

http://localhost:3000 で Learning Locker の Dashboard にアクセスできる

## Jupyter Notebook の起動

```
make start-jupyter
```

http://localhost:8888 で Jupyter Notebook にアクセスできる

## LRS (Learning Locker) の設定

### LRSの作成

Settings->Storesから新しいLRSを作成

Clientにuserとkeyが出力されるので控えておく

### Logstore xAPIの設定

http://localhost/admin/settings.php?section=logsettingxapi にアクセス

username と key に先ほど控えた文字列を貼り付け

endpointにはこちらを指定する

http://learninglocker:8081/data/xAPI/statements

この状態でmoodleを操作するとログがLRSに保存されるはず

上記でうまくいかない人向けのデバッグ用のコマンド

```
curl -X POST http://learninglocker:8081/data/xAPI/statements \
     -H "Content-Type: application/json" \
     -H "X-Experience-API-Version: 1.0.3" \
     -H "Authorization: Basic ZmZmM2IzODczMTI5NjI5YTBiZmVlNWE4YmQ4MzU1YzNjYjA5ZTk3NzowMjNkYzYyNTY1ZWZjZTA1MTVhMDE1YzEwYmFmNDA4OWMzMTlhZTkz" \
     -d '{
       "actor": {
         "mbox": "mailto:example@example.com",
         "name": "Example Learner",
         "objectType": "Agent"
       },
       "verb": {
         "id": "http://adlnet.gov/expapi/verbs/completed",
         "display": {"en-US": "completed"}
       },
       "object": {
         "id": "http://example.com/activities/course1",
         "definition": {
           "name": {"en-US": "Example Course"},
           "type": "http://adlnet.gov/expapi/activities/course"
         },
         "objectType": "Activity"
       }
     }'
```
