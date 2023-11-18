# Introduction 
**After learning Netty, I decided to write a lightweight RPC framework based on Netty, Zookeeper, and Spring.**


# Features
- **Support long connection**
- **Supports asynchronous calls**
- **Support heartbeat detection**
- **Supports JSON serialization**
- **Close to zero configuration, calling based on annotations**
- **Implementing service registration center based on Zookeeper**
- **Support dynamic management of client connections**
- **Support client service monitoring and discovery functions**
- **Supports server-side service registration function**
- **Based on Netty4.X version**

# Quick Start
### Server end development
- **Add your own Service under Service on the server side and add the @Service annotation**
	
	<pre>
	@Service
	public class TestService {
		public void test(User user){
			System.out.println("调用了TestService.test");
		}
	}
	</pre>
	
- **Generate a service interface and generate a class that implements the interface**
	
	###### The interface is as follows
	<pre>
	public interface TestRemote {
		public Response testUser(User user);  
	}
	</pre>
	###### The implementation class is as follows. Add the @Remote annotation to your implementation class. This class is where you actually call the service. You can generate any form of Response you want to return to the client.
	
	<pre> 
	@Remote
	public class TestRemoteImpl implements TestRemote{
		@Resource
		private TestService service;
		public Response testUser(User user){
			service.test(user);
			Response response = ResponseUtil.createSuccessResponse(user);
			return response;
		}
	}	
	</pre>


### client development
- **Generate an interface on the client, which is the interface you want to call**
	
	<pre>
	public interface TestRemote {
		public Response testUser(User user);
	}
	</pre>

### Use
- **Generate a property in the form of an interface where you want to call it, and add the @RemoteInvoke annotation to the property**
	
	<pre>
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration(classes=RemoteInvokeTest.class)
	@ComponentScan("\\")
	public class RemoteInvokeTest {
		@RemoteInvoke
		public static TestRemote userremote;
		public static User user;
		@Test
		public void testSaveUser(){
			User user = new User();
			user.setId(1000);
			user.setName("张三");
			userremote.testUser(user);
		}
	}	
	</pre>

### Result
- **Results of 10,000 calls**
![Markdown](https://s1.ax1x.com/2018/07/06/PZMMBF.png)

- **Results of 100,000 calls**
![Markdown](https://s1.ax1x.com/2018/07/06/PZM3N9.png)

- **Results of 1,000,000 calls**
![Markdown](https://s1.ax1x.com/2018/07/06/PZMY1x.png)



# Overview

![Markdown](https://s1.ax1x.com/2018/07/06/PZK3SP.png)
