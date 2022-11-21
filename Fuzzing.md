Python random byte generator:
```python
import random
def gen_string():
	for i in range(random.randint(0,100)):
		num = random.randint(0,254)
		ret += str(chr(num)).encode()
	return ret
```