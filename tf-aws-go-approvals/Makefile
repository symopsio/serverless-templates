APP=sym-approvals

build:
	export GO111MODULE=on
	env GOOS=linux go build -ldflags="-s -w" -o bin/approve approve/main.go
	env GOOS=linux go build -ldflags="-s -w" -o bin/expire expire/main.go

clean:
	rm -rf ./bin ./vendor Gopkg.lock ./dist

dist: build
	mkdir -p dist
	zip dist/${APP}.zip bin/approve bin/expire

deploy: dist
	aws lambda update-function-code --function-name ${APP}-approve --zip-file fileb://dist/${APP}.zip --query '{Name: FunctionName, Revision: RevisionId}'
	aws lambda update-function-code --function-name ${APP}-expire --zip-file fileb://dist/${APP}.zip --query '{Name: FunctionName, Revision: RevisionId}'

.PHONY: build clean dist deploy
