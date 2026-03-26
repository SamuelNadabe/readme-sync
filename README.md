# Readme Sync

Um módulo para transclusão de códigos do mesmo repositório para o README com o objetivo de aplicar práticas de **Doc-as-Code**.

> [!NOTE]
> **WIP - Work in Progress** > Este projeto antecipa conceitos do currículo acadêmico de **CST em Telemática**.

<!-- COPY: scripts/sync_readme.py TYPE: python -->
```python
import re
import os
import sys

pattern = re.compile(
    r'<!--\s*COPY:\s*(.*?)\s*TYPE:\s*(.*?)\s*-->\n(?:```.*?\n.*?\n```)?',
    re.DOTALL
)

# script_dir = os.path.dirname(__file__)

def load_file(file_path):
	with open(file_path, mode = 'r', encoding = 'utf-8') as f:
		content = f.read()
	return content

def save_file(file_path, content):
	with open(file_path, mode = 'w', encoding = 'utf-8') as f:
		f.write(content)

# def abs_path(path):
# 	relative_path = os.path.join(script_dir, path)
# 	return os.path.abspath(relative_path)

def sub(match):
	path = match.group(1).strip()
	lang = match.group(2).strip()

	#file_path = abs_path(path)
	file_content = load_file(path)

	return (
		f'<!-- COPY: {path} TYPE: {lang} -->\n'
		f'```{lang}\n{file_content}\n```'
	)

if __name__ == '__main__':
	readme_path = os.getenv('README_PATH', 'README.md')

	assert os.path.exists(readme_path), f'Unable to find README in {readme_path}!'
	
	readme_content = load_file(readme_path)

	readme_content = pattern.sub(sub, readme_content)

	save_file(readme_path, readme_content)
```
`<!-- COPY: scripts/sync_readme.py TYPE: python -->`

O script python acima busca por tags específicas e injeta o conteúdo do arquivo definido em `COPY` com o tipo correspondente ao `TYPE` em um bloco de código.

https://github.com/SamuelNadabe/readme-sync/blob/95461ae3cd35be0e36e364070ca5049a4b5ff807/scripts/sync_readme.py#L1-L46