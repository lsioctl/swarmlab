```
git clone https://github.com/coreos/coreos-vagrant.git
```

Changes in Vagrantfile:

```
$ git diff
diff --git a/Vagrantfile b/Vagrantfile
index d9b6c86..7aa907e 100644
--- a/Vagrantfile
+++ b/Vagrantfile
@@ -167,6 +167,11 @@ Vagrant.configure("2") do |config|
         if File.exist?(IGNITION_CONFIG_PATH)
           config.ignition.path = 'config.ign'
         end
+        # LS: added this to work with unsecure docker registry
+        # TODO: automate with laptop network IP
+        config.vm.provision :file, :source => "docker-daemon.json", :destination => "/tmp/docker-daemon.json"
+        config.vm.provision :file, :source => "docker-startup_options.conf", :destination => "/tmp/docker-startup_options.conf"
+        config.vm.provision :shell, :inline => "mkdir -p -m 700 /etc/docker; mv /tmp/docker-daemon.json /etc/docker/daemon.json; mkdir -p /etc/systemd/system/docker.service.d; cp /tmp/docker-startup_options.conf /etc/systemd/system/docker.service.d/startup_options.conf; systemctl daemon-reload; systemctl stop docker; systemctl start docker", :privileged => true
       end
     end
   end
```

copy the files here

```
git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   Vagrantfile

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	docker-daemon.json
	docker-startup_options.conf

no changes added to commit (use "git add" and/or "git commit -a")
```
