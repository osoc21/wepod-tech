# Pod Meta Data

Until know we've been stricly using the folder structure to get our info. This should also work for ACL (as this concept is "Solid native") but may be limiting for:

- Storing metadata (time, location, tags)
- Querying

The "logical" solution to this is to maintain an "index" of the files metadata.

## Format

That does not matter much. My examples here use JSON, could easily be JSON-LD (the linked data flavor of JSON) or RDF.

## Structure

What we want here is for each image:

- The image URL
- Path
- Possibly its ACL or at least private/public
- Title
- Time
- Location (GPS wise, not folder wise)
- Tags

So could be something like:

```json
[
  {
    "webId": "https://pod.inrupt.com/martinvanaken/WePod/Holidays/img-1042.jpeg",
    "path": "WePod/Holidays",
    "time": "2021-06-07 10:28",
    "access": "private",
    "title": "Walk in the parc",
    "tags": ["holidays", "friends", "nature"],
    "location": { "latitude": 20.48, "longitude": 10.44 }
  }, {
    "webId": ...
  }
]
```

Assuming the existence of such a file at the top level of the WePod folder can simplify a lot the work:

- Check for the file
- Download as an array
- Download the images themselves when needed
- Can show all useful info easily
- Make search/filtering easy


## Update

For this to work, we'll need to update it when an image is uploaded or modified (ex: tag added, ACL changed), but this is not that much work:

- On upload, ask for some info and/or extract from the image (see EXIF)
  - Update the array in the state
  - Update the file with a Solid call
- On update (ex: tags), same

## Alternative

This could also work with one metadata file per image. 

Pro: easier updates
Cons: no easy option for seach/filtering/etc (at least without communica, more on this later)