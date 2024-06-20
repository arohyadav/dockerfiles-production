# Don't use ADD for copying files. Add can be used for downloading files or software from the internet

RUN npm install && npm cache --clean force
# If you do npm cache clean force in the next layer it will not delete the cache it only has ready access to the previous layer

# Least privilege: Using node user
You must add user to run in non-root mode befor CMD in dockerfile

2) Do this after apt/apk and npm i -g (need root permissions)

3) Do this before npm install

4) May cause permissions with write access

5) chown node:node , chmod 

USER node
# user node doesnt execute every docker cmd after in the dockerfile
# only the RUN cmd, CMD cmd, ENTRYPOINT cmd, only those three cmds will behave as node user cmd 
# WORKDIR will always create directories with root permissions, so we go to go their manually and change permissions

# Set permissions on app dir
RUN mkdir app && chown -R node:node .

# For production images ( choose one option betweeen 2 and 3)
1) Use multi-stage builds
2) prod stages npm i --only=production and dev stages npm i --only=development
3) Using npm ci to speed up builds ( We have to ensure NODE_ENV is set)