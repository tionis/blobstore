# blobstore
A blobstore using github for authentication to be used for file sharing
## Design
### Authentication
Authentication is done using either oAuth2 via Github or using a github personal access token.
If using oAuth2 login, team memberships are synchronized every hour but can be rotated by renewing the session cookie or by the instance admin incrementing the cookie version.
If using personal access token authentication the token should only be used to acquire a cookie as requests using personal access tokens directly are heavily rate limited. Team memberships are synchronized during cookie creation or renewal.

### Data Model
Users can upload blobs, construct trees or update references.
Blobs are just data with some optional metadata like Mime Type and are addressed with a hash generated from the data+metadata.  
Trees are a list of pointers to other objects (like other trees or blobs) and are rendered as such when served.  
Each entry in a tree's list can have a permission object attached.  
A permission object is a list of users and groups that each have access to the ressource.
References are mutable keys that can point to other objects or external urls for url shortening.
References also have a list of permission attached with the addition of allowing writes if the Permission Object allows so.
Each user has a secret root tree that is used to prevent blobs from being garbage collected
If enabled users can host websites using the domain $USER_NAME.$ROOT_DOMAIN. This website contains the content of the user's root tree and uses the attached permissions to limit access.
