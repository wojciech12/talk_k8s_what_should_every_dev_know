## Docker Secrets

	echo "super_secret" > secret.txt
	DOCKER_BUILDKIT=1 docker build --no-cache \
	    --progress=plain \
	    --secret id=mysecret,src=secret.txt .
