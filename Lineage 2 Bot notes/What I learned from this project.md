1.If you import into main.py a variable 'running' directly:
```python 
from bot_utils import running
```
and then change its value in bot_utils.py, the value of `running` won't change its value in main.py, it will change its value only in bot_utils.py

This happens because when you import `running` directly, you copy the **reference** to the object `running` with its current value. When you change its value you create a new object in bot_utils.py namespace, but the `running` in main.py namespace still points to the old object with its old value

- If that object is immutable, you cannot change it in place, so rebinding `running` in the original module creates a new object, but the imported name still points to the old object.
- If that object is mutable, you can mutate it in place (e.g., `running.append(...)` if it were a list), and the change will be visible everywhere the object is referenced.

My solution to this was to access the value of `running` from bot_utils.py namespace:

```python
import bot_utils
		
if not bot_utils.running:
	print('not running')
```
This code will ensure that I always check the most up-to-date value

I could also solve this problem by importing a mutable object like dict:
```python
bot_utils.py
state = {running: False}

main.py
from bot_utils import state
if not state[running]:
	print('not running')
```
Because when you import a muttable object in python, a name is created in your current namespace that holds a refference to the original object, this way any modifications you made to the original object through either refference will affect the object 