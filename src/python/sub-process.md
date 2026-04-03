```py
def run(command: List[str]):
    try:
        process = subprocess.Popen(command)
        return process.wait()
    except Exception as exception:
        print(exception, file=sys.stderr)
        return 0
```
