---
layout: post
title: "[java] rabbitmq로 파일 보내기"
description: "rabbitmq로 폴더 모니터링을 곁드린 파일 보내기"
date: 2021-11-01
tags: [java, rabbitmq]
comments: true
share: true

---

rabbitmq를 이용해서 파일을 보내봤다. 근데 이제 파일 모니터링을 곁드린.

--- 

WatchService in java
java```
package com.swh.rabbitmq.project;

import java.io.IOException;
import java.nio.file.FileSystems;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardWatchEventKinds;
import java.nio.file.WatchEvent;
import java.nio.file.WatchKey;
import java.nio.file.WatchService;

public class WatchDirectory {

	public static void main(String[] args) throws IOException, InterruptedException {
		WatchService watchService
		= FileSystems.getDefault().newWatchService();

		Path path = Paths.get(System.getProperty("user.home"));

		path.register(
				watchService, 
				StandardWatchEventKinds.ENTRY_CREATE, 
				StandardWatchEventKinds.ENTRY_DELETE, 
				StandardWatchEventKinds.ENTRY_MODIFY);

		WatchKey key;
		while ((key = watchService.take()) != null) {
			for (WatchEvent<?> event : key.pollEvents()) {
				System.out.println(
						"Event kind:" + event.kind() 
						+ ". File affected: " + event.context() + ".");
			}
			key.reset();
		}
	}
}
```

--- 
