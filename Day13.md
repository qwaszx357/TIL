# Day13 2022-04-28

## AI 플랫폼을 활용한 웹서비스 개발

### Team Workshop

1. 주제 선정

   - 수험번호를 입력하면 이름, 시험성적을 확인하는 프로그램을 만든다.
   - UML

   <img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220427205445768.png" alt="image-20220427205445768" style="zoom:50%;" />

2. 테이블 설계
   - 학생 정보를 넣을 STUDENT 테이블과 시험과목 정보를 넣을 SUBJECT 테이블을 만든다.

```mysql
CREATE TABLE STUDENT(
	id int PRIMARY KEY,
    name VARCHAR(20),
    score int
);

CREATE TABLE SUBJECT(
	code int PRIMARY KEY,
    subject VARCHAR(30),
    room VARCHAR(20)
);
```

3. 프로젝트 생성

   1. day15 프로젝트를 생성한다.
   2. frame 패키지 안에 Dao 클래스를 만들어 sql과 연동한다.

   ```java
   package frame;
   
   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.util.List;
   
   public abstract class Dao<K, V> {
   
   	// MySQL Connect
   	String url = "jdbc:mysql://192.168.35.191:3306/exam?serverTimezone=Asia/Seoul";
   	String mid = "admin1";
   	String mpwd = "111111";
   	
   	public Dao() {
   		// MySQL JDBC Driver Loading
   		try {
   			Class.forName("com.mysql.cj.jdbc.Driver");
   			System.out.println("MySQL JDBC Driver Loading...");
   		} catch (ClassNotFoundException e) {
   			e.printStackTrace();
   		}
   		
   	}
   	
   	public Connection getConnection() throws SQLException {
   		Connection con = null;
   		con = DriverManager.getConnection(url, mid, mpwd);
   		return con;
   	}
   	
   	public void close(Connection con) {
   		if (con != null) {
   			try {
   				con.close();
   			} catch (SQLException e) {
   				e.printStackTrace();
   			}
   		}
   	}
   	
   	public void close(PreparedStatement ps) {
   		if (ps != null) {
   			try {
   				ps.close();
   			} catch (SQLException e) {
   				e.printStackTrace();
   			}
   		}
   	}
   	public void close(ResultSet rest) {
   		if(rest != null) {
   			try {
   				rest.close();
   			} catch (SQLException e) {
   				e.printStackTrace();
   			}
   		}
   	}
   	
   	public abstract void insert(V v) throws Exception;
   	public abstract void update(V v) throws Exception;
   	public abstract void delete(K k) throws Exception;
   	public abstract V select(K k) throws Exception;
   	public abstract List<V> select() throws Exception;
   
   }
   ```

   3. Sql 클래스 안에 sql 문을 작성한다.

   ```java
   package frame;
   
   public class Sql {
   
   	public static String insertStd = "INSERT INTO STUDENT VALUES(?, ?, ?)";
   	public static String deleteStd = "DELETE FROM STUDENT WHERE id = ?";
   	public static String updateStd = "UPDATE STUDENT SET name=?, score=? WHERE id=?";
   	public static String selectStd = "SELECT * FROM STUDENT WHERE id = ?";
   	public static String selectAllStd = "SELECT * FROM STUDENT";
   	
   	public static String insertSbj = "INSERT INTO SUBJECT VALUES(?, ?, ?)";
   	public static String deleteSbj = "DELETE FROM SUBJECT WHERE code = ?";
   	public static String updateSbj = "UPDATE SUBJECT SET subject=?, room=? WHERE code=?";
   	public static String selectSbj = "SELECT * FROM SUBJECT WHERE code = ?";
   	public static String selectAllSbj = "SELECT * FROM SUBJECT";
   }
   ```

   4. vo 패키지를 만든 뒤, StudendVo와 SubjectVo 클래스를 만든다.
      - 변수 선언한다.
      - construct를 만든다.
      - getter와 setter를 만든다.
      - toString을 만든다.
   5. dao 패키지를 만들어 StudendDao와 SubjectDao 클래스를 만든다.
      - CRUD 사용자 정의 함수를 만든다.

   ```java
   @Override
   public void insert(StudentVo v) throws Exception {
       // Connection
       Connection con = null;
       PreparedStatement ps = null;
       try {
           con = getConnection();
           ps = con.prepareStatement(Sql.insertStd);
           ps.setInt(1, v.getId());
           ps.setString(2, v.getName());
           ps.setInt(3, v.getScore());
           ps.executeUpdate();
       } catch (Exception e) {
           throw new Exception("사용자 입력 오류");
       } finally {
           close(ps);
           close(con);
       }
   }
   
   @Override
   public void update(StudentVo v) throws Exception {
       // Connection
       Connection con = null;
       PreparedStatement ps = null;
       try {
           con = getConnection();
           ps = con.prepareStatement(Sql.updateStd);
           ps.setString(1, v.getName());
           ps.setInt(2, v.getScore());
           ps.setInt(3, v.getId());
           ps.executeUpdate();	
       } catch (Exception e) {
           throw e;
       } finally {	
           close(ps);
           close(con);
       }
   
   }
   
   @Override
   public void delete(Integer k) throws Exception {
       // Connection
       Connection con = null;
       PreparedStatement ps = null;
       try {
           con = getConnection();
           ps = con.prepareStatement(Sql.deleteStd);
           ps.setInt(1, k);
           int result = ps.executeUpdate();
           if (result != 1) {
               throw new Exception("삭제 항목이 없습니다.");
           }
       } catch (Exception e) {
           throw e;
       } finally {
           close(ps);
           close(con);
       }
   }
   
   @Override
   public StudentVo select(Integer k) throws Exception {
       StudentVo std = null;
   
       Connection con = null;
       PreparedStatement ps = null;
       ResultSet rs = null;
       try {
           con = getConnection();
           ps = con.prepareStatement(Sql.selectStd);
           ps.setInt(1, k);
           rs = ps.executeQuery();
           rs.next();
           int id = rs.getInt("id");
           String name = rs.getString("name");
           int score = rs.getInt("score");
           std = new StudentVo(id, name, score);
       } catch (Exception e) {
           throw e;
       } finally {
           close(rs);
           close(ps);
           close(con);
       }
   
       return std;
   }
   
   @Override
   public List<StudentVo> select() throws Exception {
       List<StudentVo> list = new ArrayList<StudentVo>();
   
       Connection con = null;
       PreparedStatement ps = null;
       ResultSet rs = null;
       try {
           con = getConnection();
           ps = con.prepareStatement(Sql.selectAllStd);
           rs = ps.executeQuery();
           while (rs.next()) {
               int id = rs.getInt("id");
               String name = rs.getString("name");
               int score = rs.getInt("score");
               StudentVo std = new StudentVo(id, name, score);
               list.add(std);
           }
       } catch (Exception e) {
           throw e;
       } finally {	// 이렇게 해야 오류가 나도 정상적으로 종료됨
           close(rs);
           close(ps);
           close(con);
       }
   
       return list;
   }
   ```

   6. test 패키지 안에 각 함수별 테스트 클래스를 만들어 실행시켜본다.
   7. App 클래스로 실행해본다.

4. 디렉토리 설정

5. 화면 개발

   - frame을 이용해 UI를 만든다.

   ```java
   public class UI {
   	
   	Dao<Integer, StudentVo> dao;
   	java.util.List<StudentVo> slist;
   	
   	Frame f;
   	Button b1, b2;
   	Panel p1, p2;
   	Panel main;
   	TextField tf1, tf2, tf3;
   	TextField maintf;
   	List list;
   	
   	public UI() {
   		dao = new StudentDao();
   		init();
   		make();
   		addEvent();
   	}
   	
   	public void init() {
   		f = new Frame("My Frame");
   		b1 = new Button("성적 확인");
   		b2 = new Button("전체 학생 성적 확인");
   		p1 = new Panel();
   		p2 = new Panel();
   		main = new Panel();
   		maintf = new TextField();
   		tf1 = new TextField();	// 입력할 수 있는 창
   		tf2 = new TextField();
   		tf3 = new TextField();
   		list = new List();
   	}
   	
   	public void make() {
   		p1.setBackground(Color.black);
   		p2.setBackground(Color.gray);
   		
   		p2.setLayout(new BorderLayout());
   		p2.add(list,"Center");
   		p2.add(b2, "South");
   		
   		p1.setLayout(new GridLayout(4,1));
   		p1.add(tf1);
   		p1.add(b1);	
   		p1.add(tf2);
   		p1.add(tf3);
   		
   		main.setLayout(new GridLayout(1, 2));
   		main.add(p1);
   		main.add(p2);
   		
   		f.add(main, "Center");
   		f.add(maintf, "South");
   		
   		f.setSize(500,500);
   		f.setVisible(true);
   	}
   	
   	public void addEvent() {
   		
   		list.addActionListener(new ActionListener() {
   			@Override	// 리스트에서 해당 학생을 클릭하면 합격여부가 출력된다.
   			public void actionPerformed(ActionEvent e) {	// 리스트에서 클릭하면 위치를 알 수 있음
   				int t = list.getSelectedIndex();
   				StudentVo std = slist.get(t);
   				if (std.getScore() >= 80) {
   					maintf.setText(std.getScore() + "점 합격입니다.");
   				} else {
   					maintf.setText(std.getScore() + "점 불합격입니다.");
   				}
   			}
   		});
   		
   		b1.addActionListener(new ActionListener() {	
   			@Override	// 수험번호를 입력하면 이름과 점수를 확인한다.
   			public void actionPerformed(ActionEvent e) {
   				String idd = tf1.getText();
   				int id = Integer.parseInt(idd);
   				StudentVo v = new StudentVo(id);
   				try {
   					v = dao.select(id);
   					tf1.setText("");
   					tf2.setText("이름: " + v.getName());
   					tf3.setText("점수: " + v.getScore());
   					maintf.setText(id + " Select OK");
   				} catch (Exception e1) {
   					tf1.setText("");
   					tf2.setText("");
   					tf3.setText("");
   					maintf.setText("Select Error...!!");
   				}
   			}
   		});
   		
   		b2.addActionListener(new ActionListener() {
   			@Override	// 전체 학생의 수험번호와 이름을 조회한다.
   			public void actionPerformed(ActionEvent e) {
   				try {
   					slist =dao.select();
   					for (StudentVo s : slist) {
   						String str = s.getId() + " " + s.getName();
   						list.add(str);
   						maintf.setText(slist.size()+ ": Completed !");
   					}
   				} catch (Exception e1) {
   					e1.printStackTrace();
   				}
   			}
   		});
   		
   		f.addWindowListener(new WindowAdapter() {
   			@Override	// x를 누르면 종료된다.
   			public void windowClosing(WindowEvent e) {
   				System.exit(0);
   			}
   			
   		});
   	}
   }
   ```

   6. 통합
      - 학생 정보를 넣고 다양한 반례를 넣어 확인해본다.

   <img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220427211716780.png" alt="image-20220427211716780" style="zoom:33%;" />

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220427211744810.png" alt="image-20220427211744810" style="zoom:33%;" />

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220427211810609.png" alt="image-20220427211810609" style="zoom:33%;" />

