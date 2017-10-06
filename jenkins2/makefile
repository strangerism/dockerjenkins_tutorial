build:
	@docker-compose -p jenkins_2 build
	@docker-compose -p jenkins_2 pull proxy
run:
	@./setlocation.sh
	@docker-compose -p jenkins_2 up -d nginx data master proxy
stop:
	@docker-compose -p jenkins_2 stop
clean:	stop
	@docker-compose -p jenkins_2 rm master nginx proxy
clean-data: clean
	@docker-compose -p jenkins_2 rm -v data
clean-images:
	@docker rmi `docker images -q -f "dangling=true"`

