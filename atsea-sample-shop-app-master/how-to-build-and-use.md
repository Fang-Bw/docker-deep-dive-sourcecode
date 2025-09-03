# 1. 创建swarm管理节点
```bash
# 创建管理节点
docker swarm init

Swarm initialized: current node (88pgqjlr8zamjvmbhd0sky3ub) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-01v7h3qy0jqm7afzhyng3j915oi2d1op2cp2r35fn7r8noym08-7c0r1usrv1zaikq9kwj8wngb0 192.168.65.3:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

# 2.不能再创建工作节点（不能在同一主机上创建两个swarm节点）

# 3.不演示该项目