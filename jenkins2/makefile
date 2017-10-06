build:
	@docker-compose -p jenkins_2.82 build
	@docker-compose -p jenkins_2.82 pull proxy
run:
	@./setlocation.sh
	@docker-compose -p jenkins_2.82 up -d nginx data master proxy
stop:
	@docker-compose -p jenkins_2.82 stop
clean:	stop
	@docker-compose -p jenkins_2.82 rm master nginx proxy
clean-data: clean
	@docker-compose -p jenkins_2.82 rm -v data
clean-images:
	@docker rmi `docker images -q -f "dangling=true"`

