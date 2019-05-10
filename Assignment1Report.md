##Assignment 1  
 * Using JDBC to connect MySql, implement CRUD function.  
 
 
 ```java
 public int add(User user) {
		
		try {			
			
			PreparedStatement ps = con.prepareStatement("insert into users1000 (name, age, email) values(?,?,?)");
			ps.setString(1, user.getName());
			ps.setInt(2,  user.getAge());
			ps.setString(3,  user.getEmail());
			
			return ps.executeUpdate();
			
			
		}catch(SQLException e ) {
			e.printStackTrace();
		}		
		return 0;
	}
	
	public int delete(User user) {
	    
	    try {
	        
	        PreparedStatement ps = con.prepareStatement("delete from users1000 where id = ?");
	        ps.setInt(1, user.getId());
            
	        
	        return ps.executeUpdate();
	        
	        
	    }catch(Exception e) {
	        e.printStackTrace();
	    }
	    return 0;
	}	
	
	public int delete(String name) {
	    name = name.trim();
	    String sql = "delete from users1000 where name = '" + name + "'";
	    
	    
	    try {
            
            PreparedStatement ps = con.prepareStatement(sql);
            
            return ps.executeUpdate();
            
        }catch(Exception e) {
            
            e.printStackTrace();
            
        }   
        return 0;
	}
	
	public int delete(int id) {
	    String sql = "delete from users1000 where id = " + id;
	    
	    try {
	        	        
	        PreparedStatement ps = con.prepareStatement(sql);
	        
	        return ps.executeUpdate();
	    }catch(Exception e) {
	        
	        e.printStackTrace();
	        
	    }	    
	    return 0;
	}
	
	public User get(String name) {
	    User bean = null;
	    
	    String sql = "select * from users1000 where name = ?";
	    try {
	        PreparedStatement ps = con.prepareStatement(sql);
	        ps.setString(1, name);
	        ResultSet rs = ps.executeQuery();
	        
	        if(rs.next()) {
	            bean = new User();
	            
	            int id = rs.getInt("id");
	            bean.setId(id);
	            
	            String email = rs.getString("email");
	            bean.setEmail(email);
	            
	            int age = rs.getInt("age");
	            bean.setAge(age);
	            
	            bean.setId(id);	            
	        }
	    }catch(Exception e) {
            e.printStackTrace();
        }
	    
	    return bean;
	}
	
	public List<User> list(){
	    return list(0,Short.MAX_VALUE);
	}

    public List<User> list(int start, int count) {
        // TODO Auto-generated method stub
        List<User>beans = new ArrayList<User>();
        String sql = "select * from users1000 order by id desc limit ?, ?";
        
        try {
            PreparedStatement ps = con.prepareStatement(sql);
            
            ps.setInt(1, start);
            ps.setInt(2, count);
            
            ResultSet rs = ps.executeQuery();
            
            while(rs.next()) {
                
                User bean = new User();
                
                int id = rs.getInt(1);
                bean.setId(id);
                
                String name = rs.getString("name");
                bean.setName(name);
                
                int age = rs.getInt("age");
                bean.setAge(age);
               
                String email = rs.getString("email");
                bean.setEmail(email);
                
                beans.add(bean);               
                
            }                    
        }catch(Exception e) {
            e.printStackTrace();
        }
        
        return beans;
    }
    
    public int update(User bean) {
        
        String sql = "update users1000 set name = ?, age = ?, email = ? where id = ?";
        try {
            PreparedStatement ps = con.prepareStatement(sql);
            
            ps.setString(1, bean.getName());
            ps.setInt(2, bean.getAge());
            ps.setString(3, bean.getEmail());
            ps.setInt(4, bean.getId());
            
            return ps.executeUpdate();
            
        }catch(Exception e) {
            e.printStackTrace();
        }
        return 0;        
    }
 ```
 * Using JSP to display the table  
 * 
 
 ```
 <div class="container">
    <div class="row">
    <h2 style="margin-top: 70px;">Please add user below</h2>
    
    </div>
</div>

<div class="container">
    <div class="row">
        <div class="col">
        <form method="post" action="register.jsp" style="margin-top:10px;">
            Name: <input type= "text" name="name"/>
            Age: <input type= "text" name="age"/>
            E-mail: <input type= "text" name="email"/>
            <!-- <input type="submit" value="Insert"/> -->
            <button class="btn btn-primary" type="submit" style="margin-left:10px;">Add</button>
        </form>
        </div>
    </div>
</div>

<div class="container" style="margin-top:30px;">
<table class="table table-striped">
   <thead class="thead-dark">
   
    <tr>
      <!-- <th scope="col">ID</th> -->
      <th scope="col">Name</th>
      <th scope="col">Age</th>
      <th scope="col">Email</th>
      <th scope="col">Save</th>
      <th scope="col">Edit</th>
      <th scope="col">Delete</th>
    </tr>
    
   </thead> 
   <tbody>
   <%
       while(rs.next()){
              
   %>
    <tr>
    
     <% String name = rs.getString("name"); %>
     <td style="display:none;"><%= rs.getInt("id")%></td>
     <td><%= rs.getString("name")  %> </td>
     <td><%= rs.getInt("age")      %> </td>
     <td><%= rs.getString("email") %> </td>
     <td>
        <button class="savebtn" style="border: none;background: transparent">
            <i class="fas fa-save" ></i>
        </button> 
     </td> 
     <td>
        <button class="editbtn" style="border: none;background: transparent">
            <i class="fas fa-edit" ></i>
        </button> 
     </td>
     <td>        
        <button class="delbtn" style="border: none;background: transparent">
            <i class="fas fa-trash-alt"></i>
        </button>         
     </td>
       
    </tr>
    
    <% } %>
       
   </tbody>
</table>
</div>
 ```
 * Using JQuery to implement the asynchronous refresh, don't need to sendRedirect to homepage everytime. 