# 22-oct-2018

### 4 - Bytecode processor: BINARY_MULTIPLY

```C
TARGET(BINARY_MULTIPLY) // MACRO to hide whether it's old switch case or the newer goto jump
  w = POP() // w,v,x act as general purpose registers here
  v = TOP()
  x = PyNumber_Multiply(v,w); // v and w multiplication using Number protocol
  Py_DECREF(v); // why not do it safely? because it's guarenteed at this point that they will be legal
  Py_DECREF(w); // does order of these decrements matter?
  SET_TOP(x); // what if code has some failure at this point? will it revert the process to last stage?
  if ( x != NULL ) DISPATCH(); // earlier there wasn't being done in case of switch case style; but with new one i.e. labels as values version it does some work. In case the multiplication failed, x will be NULL and then this will break and exception needs to be handled.
  break;
```

PyNumber_Multiply:
```c
PyObject * 
PyNumber_Multiply(PyObject *v, PyObject *w)
{
	PyObject *result = binary_op1(v,w,NB_SLOT(nb_multiply)); // binary_op1 is local function and tries to do multiplication
	if ( result == Py_NotImplemented){// then we try to see if one of them is a sequence and other is a number and do operation of multiplication according to that
		PySequenceMethods *mv = v->ob_type->tp_as_sequence;
		PySequenceMethods *mw = v->ob_type->tp_as_sequence;
		Py_DECREF(result);
		if( mv && mv->sq_repeat){
			return sequence_repeat(mv->sq_repeat,v,w);
		}
		else if( mw && mw->sq_repeat){
			return sequence_repeat(mw->sq_repeat, w,v);
		}
		result = binop_type_error(v,w,"*"); // if all above doesn't happen, creates new type error object and raises it internally

	}
	return result;
}
```

### 3 - .pyc files

- It uses marshal module 
  - Present in ```Include/marshal.h``` and ```Python/marshal.c```
- Starts with a magic tag documented in ```Python/import.c``` , and last modification time ```st_mtime of .py``` and python code objects serialized
- While loading modules which are pure python and haven't already been preloaded by the interpreter, first pycache directory is checked for corresponding .pyc file.

### 2 - Modules written in C (CPython source)

- All C Native modules are present in ```Modules/*module.c``` and generally end with ```module.c```
- These modules get initialized inside Py_Initialize which sits in ```Python/pythonrun.c```


### 1 - Python pre-loaded modules by interpreter

- kind of module cache. When you import modules, it basically checks whether sys.modules already have that or not.
```python
import sys
print sorted(sys.modules.keys())
```


