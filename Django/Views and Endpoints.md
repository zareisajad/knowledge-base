since we can accept multiple methods on a view we can merge actions into one single view like:

```python

@api_view(["DELETE", "PATCH"])
@permission_classes([permissions.IsAuthenticated])
def address_detail(request, address_id):

	try:
		address = Address.objects.get(id=address_id, user=request.user)
	except Address.DoesNotExist:

		return Response(
			{"detail": "Address not found"},
			status=404
		)
	
	if request.method == "DELETE":
		address.delete()
		return Response(status=204)
		
	if request.method == "PATCH":
		is_default = request.data.get("is_default")

		if is_default is True:
			Address.objects.filter(
				user=request.user,
				is_default=True
			).update(is_default=False)
			address.is_default = True
			address.save()
			
	return Response({"detail": "Address Updated"}, status=200)
```