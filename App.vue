<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
			var opened=plus.sqlite.isOpenDatabase({
				name: "mhonor",
				path: "_doc/data.db"
			});
			if(!opened){
				plus.sqlite.openDatabase({
					name: "mhonor",
					path: "_doc/data.db",
					success: function(e) {
						console.log('open or create database[mhonor] success');			
					},
					fail: function(e) {
						console.log('open or create database[mhonor] failed: ' + JSON.stringify(e));
					}
				})
			}
			let sql =
				"CREATE TABLE IF NOT EXISTS application(id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,title varchar(50),note varchar(150),create_date datetime default (datetime('now', 'localtime')));";
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
