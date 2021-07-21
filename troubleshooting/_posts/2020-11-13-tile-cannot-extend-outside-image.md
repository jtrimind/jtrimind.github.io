---

title:  "[해결법] SystemError: tile cannot extend outside image"
---

PIL.image를 사용하다가 show()를 했는데 `SystemError: tile cannot extend outside image`가 뜨는 경우가 있다.  
원인은 다양하겠지만 crop()함수를 잘못 쓰는 경우 발생할 수 있다.  

## 해결법
1. img의 size를 확인해본다. 만약 (0, 0)이라면 이미지에 이상이 있다는 것이다.  
	```python
	print(img.size)
	# (0, 0)
	```
2. img를 생성할 때 함수의 파라미터를 잘 넘겼는지 확인한다.  
   참고로 crop의 경우 (left, upper, right, lower)을 파라미터로 받는다.  
	```
	Help on method crop in module PIL.Image:

	crop(box=None) method of PIL.Image.Image instance
		Returns a rectangular region from this image. The box is a
		4-tuple defining the left, upper, right, and lower pixel
		coordinate. See :ref:`coordinate-system`.
		
		Note: Prior to Pillow 3.4.0, this was a lazy operation.
		
		:param box: The crop rectangle, as a (left, upper, right, lower)-tuple.
		:rtype: :py:class:`~PIL.Image.Image`
		:returns: An :py:class:`~PIL.Image.Image` object.
	```
