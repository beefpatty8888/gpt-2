## NOTES 
Forked from https://github.com/openai/gpt-2

This project is planned to be based 
upon https://www.linuxjournal.com/content/ai-wizard-words

```
cd [repo_clone_directory]
docker build -t openai/gpt-2-gpu:$(date +%F) -f Dockerfile.gpu .

docker images

docker run -it openai/gpt-2-gpu:$(date +%F) /bin/bash

```

