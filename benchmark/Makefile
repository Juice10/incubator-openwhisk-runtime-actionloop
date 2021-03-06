N?=100
NN?=1000

.PHONY: index.html
index.html:
	@python report.py *.test >index.html
	-rm ../index.html
	mv index.html ..

tests:
	@grep '.test:' Makefile | grep -v grep | sed -e 's/:.*//'

multipost: multipost.go
	go build -o multipost multipost.go

main_gccgo.zip: main.go
	docker run -i msciab/actionloop-gccgo-v8 -compile main <$< >$@

main_go.zip: main.go
	docker run -i openwhisk/actionloop-golang-v1.11 -compile main <$< >$@

main_swift.zip: main.swift
	docker run -i msciab/actionloop-swift-v4.2.1 -compile main <$< >$@

go.test: multipost main_go.zip
	bash test.sh $(N) $(NN) openwhisk/actionloop-golang-v1.11:b51980e main_go.zip $@
	bash test.sh $(N) $(NN) msciab/actionloop-gccgo-v8 main_gccgo.zip $@

node.test: multipost
	bash test.sh $(N) $(NN) openwhisk/nodejs6action main.js $@
	bash test.sh $(N) $(NN) openwhisk/action-nodejs-v8 main.js $@
	bash test.sh $(N) $(NN) openwhisk/action-nodejs-v10 main.js $@
	bash test.sh $(N) $(NN) msciab/actionloop-nodejs-v6.14 main.js $@

python.test: multipost
	bash test.sh $(N) $(NN) openwhisk/python2action main.py $@
	bash test.sh $(N) $(NN) openwhisk/python3action main.py $@
	bash test.sh $(N) $(NN) msciab/actionloop-python-v3.6 main.py $@

php.test: multipost
	bash test.sh $(N) $(NN) openwhisk/action-php-v7.1 main.php $@
	bash test.sh $(N) $(NN) openwhisk/action-php-v7.2 main.php $@

swift.test: multipost
	bash test.sh $(N) $(NN) openwhisk/action-swift-v3.1.1 main.swift $@
	bash test.sh $(N) $(NN) openwhisk/action-swift-v4.1 main.swift $@
	bash test.sh $(N) $(NN) msciab/actionloop-swift-v4.2.1 main_swift.zip $@

ruby.test: multipost
	bash test.sh $(N) $(NN) openwhisk/action-ruby-v2.5 main.rb $@

clean:
	-rm *.test *.init *.run

