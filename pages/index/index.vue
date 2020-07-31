<template>
	<view>
		<view class="status_bar">

		</view>
		<view>
			<uni-nav-bar :fixed="false" color="#FFFFFF" background-color="#007AFF" right-icon="scan" @clickRight="scan">
				<block slot="left">
					<image class="index_logo" src="../../static/logo-trans.png"></image>
					<text class="index_text">密码器</text>
				</block>
				<view class="input-view">
					<uni-icons class="input-uni-icon" type="search" size="22" color="#666666" />
					<input confirm-type="search" class="nav-bar-input" type="text" :value="keyWord" placeholder="输入搜索关键词" @confirm="confirm">
				</view>
			</uni-nav-bar>
			<uni-list>
				<uni-list-item class="list-item" v-for="item in dataList" :title="item.title" :note="item.note" showArrow="true"
				 thumb="../../static/uni-logo.png">

				</uni-list-item>
			</uni-list>
		</view>
	</view>
</template>

<script>
	import totp from 'totp.js'
	export default {
		data() {
			return {
				keyWord: "",
				dataList: [{
						title: "Mr.mhonor的个人博客",
						note: "wj985712588"
					},
					{
						title: "阿里云",
						note: "wj985712588@163.com"
					},
					{
						title: "腾讯云",
						note: "wj985712588"
					}
				]
			}
		},
		mounted: () => {
			plus.sqlite.selectSql({
				name: "mhonor",
				sql: "select * from application",
				success: (e) => {
					console.log(JSON.stringify(e));
				}
			})
		},
		methods: {
			confirm() {
				uni.hideKeyboard();
				if (!this.$data.keyWord) {
					uni.showToast({
						icon: "none",
						title: "请输入查询关键字"
					});
					return;
				}
				uni.showToast({
					title: '搜索:' + this.keyWord
				})
			},
			scan() {
				uni.scanCode({
					scanType: ['qrCode'],
					success: (res) => {
						// otpauth://totp/Mr.mhonor?secret=EYH5PKM67FWVLJ422EMUFNT26FUB2TDG&issuer=xxx
						var data = res.result;
						// Mr.mhonor?secret=EYH5PKM67FWVLJ422EMUFNT26FUB2TDG&issuer=xxx
						var data_noprefix = data.substr(15);
						var account_info = data_noprefix.split('?')[0];
						var param_info = data_noprefix.split('?')[1];

						var ary = param_info.split('&');
						var secret;
						for (var i = 0; i < ary.length; i++) {
							if (ary[i].startsWith('secret=')) {
								secret = ary[i].split('=')[1];
								break;
							}
						}
						if (secret) {
							console.log(secret);
							var util = new totp(totp.base32.encode(secret));
							var code = util.genOTP();
							console.log(code);
						}
					}
				})
			}
		}
	}
</script>

<style>
	.status_bar {
		height: var(--status-bar-height);
		width: 100%;
		background-color: #007AFF;
	}

	.input-view {
		/* #ifndef APP-PLUS-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		/* width: 500rpx;
 */
		flex: 1;
		background-color: #f8f8f8;
		height: 30px;
		border-radius: 12px;
		padding: 0 15px;
		flex-wrap: nowrap;
		margin: 7px 0;
		line-height: 30px;
	}

	.input-uni-icon {
		line-height: 30px;
	}

	.nav-bar-input {
		height: 30px;
		line-height: 30px;
		/* #ifdef APP-PLUS-NVUE */
		width: 370rpx;
		/* #endif */
		padding: 0 5px;
		font-size: 14px;
		background-color: #f8f8f8;
		color: #000000;
	}

	.list-item {
		height: 120rpx;
	}

	.index_logo {
		width: 28px;
		height: 28px;
	}

	.index_text {
		font-size: 14px;
	}
</style>
