<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
			plus.sqlite.openDatabase({
				name: "mhonor",
				path: "_doc/mhonor.db",
				success: function(e) {
					console.log('open or create database[mhonor] success');
					let sql =
						"CREATE TABLE IF NOT EXISTS mhonor.application(id INT PRIMARY KEY AUTOINCREMENT NOT NULL,title varchar(50),note varchar(150),create_date datetimedefault (datetime('now', 'localtime')));";
						plus.sqlite.executeSql({
							name:"mhonor",
							sql:sql,
							success:function(e){
								console.log('create table[application] success');
							},
							fail:function(e){
								console.log('create table[application] failed: '+JSON.stringify(e));
							}
						});
				},
				fail: function(e) {
					console.log('open or create database[mhonor] failed: ' + JSON.stringify(e));
				}
			})
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
