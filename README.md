package frames.base;

import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;

import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.table.DefaultTableModel;

import messangerUser.base.MessangerUserDAO;

public class PrivateMsgFrame extends JFrame {

	private JTextField textprvtmsg;
	public JTextArea textprvtarea;

	public PrivateMsgFrame() {
		setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
		getContentPane().setLayout(null);

		JLabel lblNewLabel = new JLabel("msgs:");
		lblNewLabel.setBounds(10, 11, 46, 14);
		getContentPane().add(lblNewLabel);

		JScrollPane scrollPane = new JScrollPane();
		scrollPane.setBounds(10, 34, 258, 95);
		getContentPane().add(scrollPane);

		textprvtarea = new JTextArea();
		scrollPane.setViewportView(textprvtarea);

		textprvtmsg = new JTextField();
		textprvtmsg.addKeyListener(new KeyAdapter() {
			@Override
			public void keyPressed(KeyEvent arg0) {

//				JOptionPane.showMessageDialog(SingUpFrame.this,
//				((DefaultTableModel)(table.getModel())).getDataVector().elementAt(table.getSelectedRow()));

//		JOptionPane.showMessageDialog(SingUpFrame.this,
//				((DefaultTableModel)(table.getModel())).getValueAt(table.getSelectedRow(),0));

				if (arg0.getKeyCode() == KeyEvent.VK_ENTER) {

					MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.index)
							.setMessage(MessangerUserDAO.mdao.getId() + "-" + MessangerUserDAO.mdao.getName() + " : "
									+ textprvtmsg.getText());

					MessangerUserDAO.mdao
							.updateMessangerUser(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.index));

					MessangerUserDAO.mdao
							.getAllmessangerUser(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.index));

					String[] s = MessangerUserDAO.mdao.getMessage().split("-");
					// JOptionPane.showMessageDialog(PrivateMsgFrame.this,MessangerUserDAO.mdao.varmi);
					if (s[0].equals(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.index).getId() + "")) {
						textprvtarea.setText(MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.varmi][3] + "\n"
								+ MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.index][3]);
					} else {
						textprvtarea.setText(textprvtarea.getText() + "\n"
								+ MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.index][3]);
					}

					// textprvtarea.setText(textprvtarea.getText()+"\n"+MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.index).getMessage());
					textprvtmsg.setText("");

					SingUpFrame.dtm = new DefaultTableModel(MessangerUserDAO.mdao.data, MessangerUserDAO.mdao.columuns);
					SingUpFrame.table.setModel(SingUpFrame.dtm);
					// SingUpFrame.table.setVisible(true);

				}

			}
		});
		textprvtmsg.setBounds(10, 165, 258, 20);
		getContentPane().add(textprvtmsg);
		textprvtmsg.setColumns(10);

		JLabel lblNewLabel_1 = new JLabel("say:");
		lblNewLabel_1.setBounds(10, 140, 46, 14);
		getContentPane().add(lblNewLabel_1);

		setBounds(200, 200, 300, 240);
	}
}
package frames.base;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.sql.SQLException;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.Timer;
import javax.swing.table.DefaultTableModel;

import messangerUser.base.MessangerUserDAO;

public class SingUpFrame extends JFrame {

	// private static MessangerUser staticuser;
	private JTextField textid;
	private JTextField textname;
	private JTextField textmsg;

	public static JTable table;
	public static DefaultTableModel dtm;
	public static JTextArea textArea;

	public PrivateMsgFrame pmf = new PrivateMsgFrame();
	public Timer timer;

	public SingUpFrame() {

		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		getContentPane().setLayout(null);
		setBounds(100, 100, 503, 481);

		JScrollPane scrollPane = new JScrollPane();
		scrollPane.setBounds(10, 201, 468, 159);
		getContentPane().add(scrollPane);

		textArea = new JTextArea();
		scrollPane.setViewportView(textArea);
		textArea.setVisible(false);
		// textArea.setEnabled(false);

		textid = new JTextField();
		textid.setBounds(49, 43, 86, 20);
		getContentPane().add(textid);
		textid.setColumns(10);

		textname = new JTextField();
		textname.setBounds(49, 12, 86, 20);
		getContentPane().add(textname);
		textname.setColumns(10);

		JLabel lblNewLabel = new JLabel("user:");
		lblNewLabel.setBounds(10, 14, 46, 14);
		getContentPane().add(lblNewLabel);

		JLabel lblNewLabel_1 = new JLabel("id:");
		lblNewLabel_1.setBounds(10, 45, 46, 14);
		getContentPane().add(lblNewLabel_1);

		JScrollPane scrollPane_1 = new JScrollPane();
		scrollPane_1.setBounds(145, 12, 333, 151);
		getContentPane().add(scrollPane_1);

		table = new JTable();
		table.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {

				pmf.setVisible(true);

				MessangerUserDAO.mdao.index = MessangerUserDAO.mdao.getMessangerUser(
						Integer.parseInt(((DefaultTableModel) (SingUpFrame.table.getModel()))
								.getValueAt(SingUpFrame.table.getSelectedRow(), 0) + ""),
						((DefaultTableModel) (SingUpFrame.table.getModel()))
								.getValueAt(SingUpFrame.table.getSelectedRow(), 1) + "");

//				JOptionPane.showMessageDialog(SingUpFrame.this,index);

				pmf.setTitle(MessangerUserDAO.mdao.getId() + "to"
						+ MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.index).getId());
				pmf.textprvtarea.setText("");

			}
		});
		table.setVisible(false);
		scrollPane_1.setViewportView(table);

		JButton btnnew = new JButton("new");
		btnnew.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent arg0) {
				if (MessangerUserDAO.mdao == null) {
					try {

						MessangerUserDAO.getIstance();

						MessangerUserDAO.mdao.setName(textname.getText());
						MessangerUserDAO.mdao.setUsing(true);
						MessangerUserDAO.mdao.insertMessangerUser(MessangerUserDAO.mdao);
						MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);
						MessangerUserDAO.mdao
								.setId(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.users.size() - 1).getId());
						MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);

						MessangerUserDAO.mdao.varmi = MessangerUserDAO.mdao.users.size() - 1;
//						System.out.println(MessangerUserDAO.mdao.varmi);
//						MessangerUserDAO.mdao.varmi = MessangerUserDAO.mdao.getMessangerUser(MessangerUserDAO.mdao.getId(),MessangerUserDAO.mdao.getName());
//						System.out.println(MessangerUserDAO.mdao.varmi);
						if (MessangerUserDAO.mdao.varmi == -2) {
							JOptionPane.showMessageDialog(SingUpFrame.this, "bu id ile bir kullanici yok");
							MessangerUserDAO.mdao = null;
						} else if (MessangerUserDAO.mdao.varmi == -1) {
							JOptionPane.showMessageDialog(SingUpFrame.this, "hatali kullanici");
							MessangerUserDAO.mdao = null;
						} else if (MessangerUserDAO.mdao.varmi >= 0) {

							// MessangerUserDAO.mdao.setName(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.varmi).getName());
							// MessangerUserDAO.mdao.setId(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.varmi).getId());
							MessangerUserDAO.mdao.setMessage(
									MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.varmi).getMessage());

							MessangerUserDAO.mdao.setMessages(
									MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.varmi).getMessages());
							// MessangerUserDAO.mdao.setUsing(true);

							MessangerUserDAO.mdao.updateMessangerUser(MessangerUserDAO.mdao);
							MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);

							textArea.setVisible(true);
							textArea.setEnabled(true);
							textmsg.setVisible(true);
							textmsg.setEnabled(true);
							pmf.setVisible(false);

							table.setVisible(true);
							setTitle(MessangerUserDAO.mdao.getId() + "");

							ActionListener a = new ActionListener() {
								@Override
								public void actionPerformed(ActionEvent arg0) {
									// TODO Auto-generated method stub

									try {
										String lastmessage = MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.varmi][3];
										MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);
										MessangerUserDAO.mdao.getAllmessages(MessangerUserDAO.mdao);
										if (!lastmessage
												.equals(MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.varmi][3])) {
											String[] sndr = MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.varmi][3]
													.split(":");
											JOptionPane.showMessageDialog(SingUpFrame.this,
													sndr[0] + "kullanicisindan mesajınız var");

										}
										System.out.println(MessangerUserDAO.mdao.getId());
									} catch (Exception e) {
									}

								}
							};
							if (timer == null) {
								timer = new Timer(5000, a);
								// timer.setRepeats(false);
								timer.start();
							}

						}

//						//MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);
//						MessangerUserDAO.mdao.setName(textname.getText());
//						MessangerUserDAO.mdao.setUsing(true);
//						MessangerUserDAO.mdao.insertMessangerUser(MessangerUserDAO.mdao);
//						MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);
//						MessangerUserDAO.mdao
//								.setId(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.users.size() - 1).getId());
//						MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);
//
//						textArea.setVisible(true);
//						textArea.setEnabled(true);
//						textmsg.setVisible(true);
//						textmsg.setEnabled(true);
//
//						table.setVisible(true);
//						setTitle(MessangerUserDAO.mdao.getId() + "");

					} catch (ClassNotFoundException | SQLException e) {
						// TODO Auto-generated catch block
						JOptionPane.showMessageDialog(SingUpFrame.this, "baglanti hatasi");
						MessangerUserDAO.mdao = null;
						e.printStackTrace();
					}
				} else {
					JOptionPane.showMessageDialog(SingUpFrame.this, "kullanici mevcut");
				}

			}
		});
		btnnew.setBounds(49, 106, 89, 23);
		getContentPane().add(btnnew);

		JButton btnjoin = new JButton("join");
		btnjoin.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {

				if (!textid.getText().matches("[0-9]") && textid.getText().matches("")) {
					JOptionPane.showMessageDialog(SingUpFrame.this, "id giriniz");
				} else {

					try {
						MessangerUserDAO.getIstance();

						MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);

						MessangerUserDAO.mdao.varmi = MessangerUserDAO.mdao
								.getMessangerUser(Integer.parseInt(textid.getText()), textname.getText());
						if (MessangerUserDAO.mdao.varmi == -2) {
							JOptionPane.showMessageDialog(SingUpFrame.this, "bu id ile bir kullanici yok");
							MessangerUserDAO.mdao = null;
							timer = null;
						} else if (MessangerUserDAO.mdao.varmi == -1) {
							JOptionPane.showMessageDialog(SingUpFrame.this, "hatali kullanici");
							// MessangerUserDAO.mdao = null;
						} else if (MessangerUserDAO.mdao.varmi >= 0) {

							MessangerUserDAO.mdao
									.setName(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.varmi).getName());
							MessangerUserDAO.mdao
									.setId(MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.varmi).getId());
							MessangerUserDAO.mdao.setMessage(
									MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.varmi).getMessage());

							MessangerUserDAO.mdao.setMessages(
									MessangerUserDAO.mdao.users.get(MessangerUserDAO.mdao.varmi).getMessages());
							MessangerUserDAO.mdao.setUsing(true);

							MessangerUserDAO.mdao.updateMessangerUser(MessangerUserDAO.mdao);
							MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);

							textArea.setVisible(true);
							textArea.setEnabled(true);
							textmsg.setVisible(true);
							textmsg.setEnabled(true);
							pmf.setVisible(false);

							table.setVisible(true);
							setTitle(MessangerUserDAO.mdao.getId() + "");

							ActionListener a = new ActionListener() {
								@Override
								public void actionPerformed(ActionEvent arg0) {
									// TODO Auto-generated method stub

									try {
										String lastmessage = MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.varmi][3];
										MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);
										MessangerUserDAO.mdao.getAllmessages(MessangerUserDAO.mdao);
										if (!lastmessage
												.equals(MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.varmi][3])) {
											String[] sndr = MessangerUserDAO.mdao.data[MessangerUserDAO.mdao.varmi][3]
													.split(":");
											JOptionPane.showMessageDialog(SingUpFrame.this,
													sndr[0] + "kullanicisindan mesajınız var");

										}
										System.out.println(MessangerUserDAO.mdao.getId());
									} catch (Exception e) {
									}

								}
							};
							if (timer == null) {
								timer = new Timer(5000, a);
								// timer.setRepeats(false);
								timer.start();
							}

						}

					} catch (ClassNotFoundException | SQLException e1) {
						// TODO Auto-generated catch block
						JOptionPane.showMessageDialog(SingUpFrame.this, "baglanti hatasi");
						MessangerUserDAO.mdao = null;
						e1.printStackTrace();
					}
				}

			}
		});
		btnjoin.setBounds(49, 74, 89, 23);
		getContentPane().add(btnjoin);

		JButton btnNewButton = new JButton("on/off");
		btnNewButton.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				if (MessangerUserDAO.mdao != null) {
					if (MessangerUserDAO.mdao.isUsing() == true) {

						MessangerUserDAO.mdao.setUsing(false);
						MessangerUserDAO.mdao.updateMessangerUser(MessangerUserDAO.mdao);
						MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);

						textArea.setVisible(true);
						textArea.setEnabled(false);
						textmsg.setVisible(true);
						// textmsg.setEnabled(false);

						table.setVisible(true);

						try {
							MessangerUserDAO.mdao.getC().close();
							timer.stop();
						} catch (SQLException e1) {
							// TODO Auto-generated catch block
							e1.printStackTrace();
						}

					} else {
						MessangerUserDAO.mdao.setUsing(true);
						try {
							// MessangerUser usr =
							// MessangerUserDAOInterface.getMessangerUser(staticuser.getId());
							MessangerUserDAO.mdao.connectionopen();
							timer.start();

							MessangerUserDAO.mdao.updateMessangerUser(MessangerUserDAO.mdao);
							MessangerUserDAO.mdao.getAllmessangerUser(MessangerUserDAO.mdao);

							textArea.setVisible(true);
							textArea.setEnabled(true);
							textmsg.setVisible(true);
							textmsg.setEnabled(true);

							table.setVisible(true);

						} catch (SQLException e1) {
							// TODO Auto-generated catch block
							e1.printStackTrace();
						}
					}
				}

			}
		});
		btnNewButton.setBounds(49, 140, 89, 23);
		getContentPane().add(btnNewButton);

		textmsg = new JTextField();
		textmsg.setEnabled(false);

		textmsg.addKeyListener(new KeyAdapter() {
			@Override
			public void keyPressed(KeyEvent e) {

				if (e.getKeyCode() == KeyEvent.VK_ENTER) {

					MessangerUserDAO.mdao.setMessages(textmsg.getText());

					MessangerUserDAO.mdao.insertMessages(MessangerUserDAO.mdao);
					MessangerUserDAO.mdao.getAllmessages(MessangerUserDAO.mdao);

					setTitle(MessangerUserDAO.mdao.getId() + "");
					textmsg.setText("");

				}

			}
		});
		textmsg.setBounds(10, 396, 468, 31);
		getContentPane().add(textmsg);
		textmsg.setColumns(10);

		JLabel lblNewLabel_2 = new JLabel("message:");
		lblNewLabel_2.setBounds(10, 371, 125, 14);
		getContentPane().add(lblNewLabel_2);

		JLabel lblNewLabel_3 = new JLabel("messagespool:");
		lblNewLabel_3.setBounds(10, 176, 125, 14);
		getContentPane().add(lblNewLabel_3);

	}
}
package messangerUser.base;

public class ConnectionManager {

	private static ConnectionManager instance;
	private String url;
	private String dbname;
	private String dbpassword;

	public static ConnectionManager getInstance() throws ClassNotFoundException {

		if (instance == null) {
			instance = new ConnectionManager();
			Class.forName("org.postgresql.Driver");
			instance.setUrl("jdbc:postgresql://yourip:port/databasename");
			instance.setDbname("yourdatabase");
			instance.setDbpassword("yourpassword");

		}

		return instance;

	}

	public String getUrl() {
		return url;
	}

	public void setUrl(String url) {

		this.url = url;
	}

	public String getDbname() {
		return dbname;
	}

	public void setDbname(String dbname) {

		this.dbname = dbname;
	}

	public String getDbpassword() {
		return dbpassword;
	}

	public void setDbpassword(String dbpassword) {

		this.dbpassword = dbpassword;
	}

}
package messangerUser.base;

public class MessangerUser {

	// public MessangerUser user;
	public String name;
	public int id;
	public String message;
	public String messages;
	public boolean using = false;

	public MessangerUser() {
	}

	public String getName() {
		return name;
	}

	public String getMessages() {
		return messages;
	}

	public void setMessages(String messages) {
		this.messages = messages;
	}

	public boolean isUsing() {
		return using;
	}

	public void setUsing(boolean using) {
		this.using = using;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}

}
package messangerUser.base;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

import javax.swing.table.DefaultTableModel;

import frames.base.SingUpFrame;

public class MessangerUserDAO extends MessangerUser implements MessangerUserDAOInterface {

	public List<MessangerUser> users = new ArrayList<>();
	public static MessangerUserDAO mdao;
	public String[][] data;
	public String[] columuns = { "id", "name", "connection", "message" };
	public ConnectionManager cm;
	public Connection c;
	public int index = -1;
	public int varmi = -2;

	public static MessangerUserDAO getIstance() throws ClassNotFoundException, SQLException {
		if (mdao == null) {
			mdao = new MessangerUserDAO();
			mdao.setCm(ConnectionManager.getInstance());
			mdao.setC(DriverManager.getConnection(mdao.cm.getUrl(), mdao.cm.getDbname(), mdao.cm.getDbpassword()));

		}
		return mdao;

	}

	public int getIndex() {
		return index;
	}

	public void setIndex(int index) {
		this.index = index;
	}

	private MessangerUserDAO() {
	}

	public ConnectionManager getCm() {
		return cm;
	}

	public void setCm(ConnectionManager cm) {
		this.cm = cm;
	}

	public Connection getC() {
		return c;
	}

	public void setC(Connection c) {
		this.c = c;
	}

	@Override
	public List<MessangerUser> getAllmessangerUser(MessangerUser m) {
		// TODO Auto-generated method stub
		users.clear();

		Statement stmt = null;
		try {

			mdao.getC().setAutoCommit(true);
			System.out.println("Opened database successfully");

			stmt = mdao.getC().createStatement();
			ResultSet rs = stmt.executeQuery("select * from messanger order by id;");

			while (rs.next()) {

				MessangerUser newuser = new MessangerUser();

				newuser.setId(rs.getInt("id"));
				newuser.setName(rs.getString("name"));
				newuser.setUsing(rs.getBoolean("connection"));
				newuser.setMessage(rs.getString("message"));

				users.add(newuser);

			}
			rs.close();
			stmt.close();

			System.out.println("Operation done successfully");
		} catch (Exception e) {
			System.err.println(e.getClass().getName() + ": " + e.getMessage());
			// System.exit(0);
		}

		data = new String[users.size()][4];
		for (int i = 0; i < users.size(); i++) {

			data[i][0] = users.get(i).getId() + "";
			data[i][1] = users.get(i).getName();
			data[i][2] = users.get(i).isUsing() + "";
			data[i][3] = users.get(i).getMessage();

		}
		SingUpFrame.dtm = new DefaultTableModel(data, columuns);
		SingUpFrame.table.setModel(SingUpFrame.dtm);

		return users;

	}

	@Override
	public void updateMessangerUser(MessangerUser m) {
		// TODO Auto-generated method stub

		Statement stmt = null;

		try {

			// cm= ConnectionManager.getInstance();
//			if (m.getC().isClosed()) {
//				m.connectionopen();
//				System.out.println("connection opend!");
//			}

			mdao.getC().setAutoCommit(true);
			System.out.println("Opened database successfully");

			stmt = mdao.getC().createStatement();
			String sql = "update messanger set name = '" + m.getName() + "', " + "connection = " + m.isUsing()
					+ ", message = '" + m.getMessage() + "' where id = " + m.getId() + ";";
			stmt.executeUpdate(sql);
			// m.getC().commit();
			stmt.close();
			// s.getCm().getC().close();

			System.out.println("Operation done successfully");
		} catch (Exception e) {
			System.err.println(e.getClass().getName() + ": " + e.getMessage());
			// System.exit(0);

		}

	}

	@Override
	public void deleteMessangerUser(MessangerUser m) {
		// TODO Auto-generated method stub

	}

	@Override
	public void insertMessangerUser(MessangerUser m) {
		// TODO Auto-generated method stub

		// System.out.println(s.getName()+s.getCm().isUsing()+s.getCm().getC()+s.getCm().getUrl()+s.getCm().getDbname()+s.getCm().getDbpassword());
		Statement stmt = null;
		try {
			// cm= ConnectionManager.getInstance();
			// s.getCm().connectionopen();
			mdao.getC().setAutoCommit(true);

			stmt = mdao.getC().createStatement();
			String sql = "insert into messanger (name,connection,message) values ('" + m.getName() + "', " + m.isUsing()
					+ ",'" + m.getMessage() + "')";
			stmt.executeUpdate(sql);

			stmt.close();
			// m.getC().commit();
			// m.getC().close();
			System.out.println("Records created successfully");
		} catch (Exception e) {
			System.err.println(e.getClass().getName() + ": " + e.getMessage());
//			try {
//				newstudent.getCm().getC().rollback();
//			} catch (SQLException e1) {
//				e1.printStackTrace();
//			}
			// System.exit(0);
		}

	}

	@Override
	public void insertMessages(MessangerUser m) {
		// TODO Auto-generated method stub
		Statement stmt = null;

		try {

			// cm= ConnectionManager.getInstance();
//			if (m.getC().isClosed()) {
//				m.connectionopen();
//				System.out.println("connection opend!");
//			}

			mdao.getC().setAutoCommit(true);
			System.out.println("Opened database successfully");

			stmt = mdao.getC().createStatement();
			String sql = "insert into messanger2 (id,name,messages) values (" + m.getId() + ", '" + m.getName() + "', '"
					+ m.getMessages() + "')";
			stmt.executeUpdate(sql);
			// m.getC().commit();
			stmt.close();
			// s.getCm().getC().close();

			System.out.println("Operation done successfully");
		} catch (Exception e) {
			System.err.println(e.getClass().getName() + ": " + e.getMessage());
			// System.exit(0);

		}
	}

	@Override
	public void getAllmessages(MessangerUser m) {
		// TODO Auto-generated method stub

		String s = "";
		Statement stmt = null;

		try {

			mdao.getC().setAutoCommit(true);
			System.out.println("Opened database successfully");

			stmt = mdao.getC().createStatement();

			ResultSet rs = stmt.executeQuery("select * from messanger2;");
			SingUpFrame.textArea.setText("");
			while (rs.next()) {

				s = rs.getInt("id") + " " + rs.getString("name") + " : " + rs.getString("messages");
				SingUpFrame.textArea.setText(SingUpFrame.textArea.getText() + "\n" + s);

			}

			System.out.println("Operation done successfully");
		} catch (Exception e) {
			System.err.println(e.getClass().getName() + ": " + e.getMessage());
			// System.exit(0);
		}

	}

	@Override
	public int getMessangerUser(int id, String name) {
		// TODO Auto-generated method stub
		int varmi = -2;
		for (int i = 0; i < users.size(); i++) {
			// System.out.println(stdnt.getName()+" "+stdnt.getId()+" ="+id);
			if (users.get(i).getId() == id) {

				varmi = -1;

				if (users.get(i).getName().equals(name)) {
					varmi = i;
					break;
				}

			}

		}

		return varmi;
	}

	@Override
	public void connectionopen() throws SQLException {
		// TODO Auto-generated method stub
		mdao.setC(DriverManager.getConnection(mdao.cm.getUrl(), mdao.cm.getDbname(), mdao.cm.getDbpassword()));
	}

}
package messangerUser.base;

import java.sql.SQLException;
import java.util.List;

public interface MessangerUserDAOInterface {

	public List<MessangerUser> getAllmessangerUser(MessangerUser m);

	public void updateMessangerUser(MessangerUser m);

	public void deleteMessangerUser(MessangerUser m);

	public void insertMessangerUser(MessangerUser m);

	public void insertMessages(MessangerUser m);

	public void getAllmessages(MessangerUser m);

	public int getMessangerUser(int id, String name);

	public void connectionopen() throws SQLException;

}
package messangerUser.base;

import java.sql.SQLException;

import frames.base.SingUpFrame;

public class Runner {
	public static void main(String[] args) throws ClassNotFoundException, SQLException {

		SingUpFrame suf = new SingUpFrame();
		suf.setVisible(true);

	}

}
