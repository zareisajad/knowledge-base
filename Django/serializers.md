
Sializers allow complex data such as querysets and model instances to be converted to native Python datatypes,

very similarly to Django's Form and ModelForm classes.

#### PrimaryKeyRelatedField

`PrimaryKeyRelatedField` may be used to represent the target of the relationship using its primary key.

For example, the following serializer:

```
class AlbumSerializer(serializers.ModelSerializer):
    tracks = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```

Would serialize to a representation like this:

```
{
    'album_name': 'Undun',
    'artist': 'The Roots',
    'tracks': [
        89,
        90,
        91,
        ...
    ]
}
```