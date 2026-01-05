
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


# [Nested relationships](https://www.django-rest-framework.org/api-guide/relations/#nested-relationships)

As opposed to previously discussed _references_ to another entity, the referred entity can instead also be embedded or _nested_ in the representation of the object that refers to it. Such nested relationships can be expressed by using serializers as fields.

If the field is used to represent a to-many relationship, you should add the `many=True` flag to the serializer field.

## [Example](https://www.django-rest-framework.org/api-guide/relations/#example)

For example, the following serializer:

```
class TrackSerializer(serializers.ModelSerializer):
    class Meta:
        model = Track
        fields = ['order', 'title', 'duration']

class AlbumSerializer(serializers.ModelSerializer):
    tracks = TrackSerializer(many=True, read_only=True)

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```