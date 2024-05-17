Spin up a instance in ec2, since lot of tools jenkins, docker, git are installed better to proceed with large instance to run smoothly

1. install git 
2. configure git 
    - Create ssh keygen new algo - ed25519 (crypotgraphic algorithm faster than rsa)
    - move it to ssh agent as it will take care of auth futher - no need to add ssh key each time for git auth
    - Add the public key to github - ssh keys 
    - check ssh -T git@github.com for auth 
3. Install java 
4. Install jenkins 
    - https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/
5. Setup jenkins with plugins and user
6. Create new pipeline project - provide git as source and also repo name and branch