#!/bin/bash
brew install --build-bottle Formula/valet-php@"$PHPV".rb
brew bottle valet-php@$PHPV --json --force-core-tap
brew install jq
bottleJson=$( cat *.json )
FILENAME=$(echo $bottleJson | jq --raw-output '."valet-php@'"$PHPV"'"."bottle".tags.'"$OS"'.filename')
echo $FILENAME
LOCALFILE=$(echo $bottleJson | jq --raw-output '."valet-php@'"$PHPV"'"."bottle".tags.'"$OS"'.local_filename')
echo $LOCALFILE
SHA256=$(echo $bottleJson | jq --raw-output '."valet-php@'"$PHPV"'"."bottle".tags.'"$OS"'.sha256')
echo $SHA256

ls -all
#         Transfer to bintray
curl -T $LOCALFILE -uhenkrehorst:$BINTRAY_KEY https://api.bintray.com/content/henkrehorst/"$PACKAGE_REPOSITORY"/php-"$PHPV"/"$BUILD_VERSION"/$FILENAME
# TRANSFER BOTTLE HASH TO AUTOMATION
curl --location --request POST $AUTOMATION_ENDPOINT'/platform/update/'"$UPDATE_ID"'' \
--header 'token: '"$APITOKEN"'' \
--form 'bottle_hash='"$SHA256"''