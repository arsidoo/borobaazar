<template>
	<footer class="footer-section">
		<div class="footer-top">
			<div class="container">
				<div class="row">
					<div class="col-md-8 footer-left">
						<div class="row">
							<div class="col-sm-2 col-md-2">
								<div class="footer-logo">
									<router-link :to="{ name: 'home' }">
										<img loading="lazy" :src="settings.footer_logo" alt="preloader" class="img-fluid"
											width="70px" />
									</router-link>
								</div>
							</div>
							<div class="col-sm-10 col-md-10 mb-5">
								<div v-html="settings.about_description"></div>
							</div>
							<div class="col-sm-4 col-md-6 col-lg-4">
								<div class="footer-widget widget-border">
									<h3>{{ lang.my_account }}</h3>
									<ul class="global-list" v-if="!authUser">
										<li>
											<router-link :to="{ name: 'login' }">{{ lang.Login }}</router-link>
										</li>
										<li>
											<router-link :to="{ name: 'register' }">{{ lang.create_account }}</router-link>
										</li>
									</ul>
									<ul class="global-list" v-if="authUser && authUser.user_type == 'customer'">
										<li>
											<router-link :to="{ name: 'dashboard' }">{{ lang.my_profile }}</router-link>
										</li>
										<li>
											<router-link :to="{ name: 'change.password' }">{{ lang.change_password }}
											</router-link>
										</li>
										<li>
											<router-link :to="{ name: 'order.history' }">{{ lang.order_history
											}}</router-link>
										</li>
										<li>
											<router-link :to="{ name: 'wishlist' }">{{ lang.my_wishlist }}</router-link>
										</li>
										<li>
											<router-link :to="{ name: 'addresses' }">{{ lang.addresses }}</router-link>
										</li>
										<li>
											<router-link :to="{ name: 'track.order' }">{{ lang.track_order }}</router-link>
										</li>
										<li>
											<router-link :to="{ name: 'gift.voucher' }">{{ lang.gift_voucher
											}}</router-link>
										</li>
									</ul>
									<ul class="global-list"
										v-else-if="authUser && (authUser.user_type == 'admin' || authUser.user_type == 'staff')">
										<li><a target="_blank" :href="getUrl('admin/dashboard')">{{ lang.dashboard }}</a>
										</li>
										<li><a target="_blank" :href="getUrl('admin/profile')">{{ lang.my_profile }}</a>
										</li>
										<li><a target="_blank" :href="getUrl('admin/password-change')">{{
											lang.change_password
										}}</a></li>
									</ul>
									<ul class="global-list" v-else-if="authUser && authUser.user_type == 'seller'">
										<li><a target="_blank" :href="getUrl('seller/dashboard')">{{ lang.dashboard }}</a>
										</li>
										<li><a target="_blank" :href="getUrl('seller/profile')">{{ lang.my_profile }}</a>
										</li>
										<li><a target="_blank" :href="getUrl('seller/password-change')">{{
											lang.change_password
										}}</a></li>
									</ul>
								</div>
							</div>
							<div class="col-sm-4 col-md-6 col-lg-4">
								<div class="footer-widget widget-border">
									<h3>{{ lang.useful_links }}</h3>
									<ul class="global-list" v-for="(link, i) in usefulLinks" :key="i">
										<li>
											<router-link :to="link.url">{{ link.label }}</router-link>
										</li>
									</ul>
								</div>
							</div>
							<div class="col-sm-4 col-md-6 col-lg-4">
								<div class="footer-widget widget-border">
									<h3>{{ lang.contact_us }}</h3>
									<div class="address">
										<ul class="global-list">
											<li>
												<h4><span class="mdi mdi-home-outline"></span>{{ lang.address }}</h4>
												<p>{{ settings.footer_contact_address }}</p>
											</li>
										</ul>
									</div>
								</div>
							</div>
						</div>
					</div>
					<div class="col-md-4 text-center">
						<div class="row">
							<div class="col-sm-6 col-md-6 mb-1">
								<img src="https://chaldn.com/asset/Egg.ChaldalWeb.Fabric/Egg.ChaldalWeb/1.0.0+Deploy-Release-283/Default/components/shared/NewFooter/images/google_play_store.png?q=low&amp;webp=1&amp;alpha=1"
									data-reactid=".1n2zjksuaji.b.2.0.0.b.2.0.0.0.0.1">
							</div>
							<div class="col-sm-6 col-md-6 mb-1">
								<img src="https://chaldn.com/asset/Egg.ChaldalWeb.Fabric/Egg.ChaldalWeb/1.0.0+Deploy-Release-283/Default/components/shared/NewFooter/images/app_store.png?q=low&amp;webp=1&amp;alpha=1"
									data-reactid=".1n2zjksuaji.b.2.0.0.b.2.0.0.1.0.1">
							</div>
							<div class="col-sm-8 col-md-12 mt-5">
								<h4><span class="mdi mdi-phone-outline"></span>
									<a :href="'tel:' + settings.footer_contact_phone">{{
										settings.footer_contact_phone
									}}</a>
								</h4>
								<h5>
									<span class="mdi mdi-email"></span>
									<a :href="'mailto:' + settings.footer_contact_email">{{
										settings.footer_contact_email }}</a>
								</h5>
							</div>
							<div class="col-sm-4 col-md-12 d-flex justify-content-center">
								<div class="social mt-5"
									v-if="settings.show_social_links && settings.show_social_links == 1">
									<ul class="global-list d-flex justify-content-center">
										<li v-if="settings.facebook_link">
											<a target="_blank" :href="settings.facebook_link"><span
													class="mdi mdi-name mdi-facebook"></span></a>
										</li>
										<li v-if="settings.twitter_link">
											<a target="_blank" :href="settings.twitter_link"><span
													class="mdi mdi-name mdi-twitter"></span></a>
										</li>
										<li v-if="settings.linkedin_link">
											<a target="_blank" :href="settings.linkedin_link"><span
													class="mdi mdi-linkedin"></span></a>
										</li>
										<li v-if="settings.instagram_link">
											<a target="_blank" :href="settings.instagram_link"><span
													class="mdi mdi-instagram"></span></a>
										</li>
										<li v-if="settings.youtube_link">
											<a target="_blank" :href="settings.youtube_link"><span
													class="mdi mdi-youtube"></span></a>
										</li>
									</ul>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!-- <div class="footer-social">
			<div class="container">
				<ul class="global-list">
					<li v-for="(menu, i) in footerMenu" :key="i">
						<a v-if="urlCheck(menu.url)" :href="menu.url">{{ menu.label }}</a>
						<router-link :to="menu.url">{{ menu.label }}</router-link>
					</li>
				</ul>
			</div>
		</div> -->

		<div class="footer-bottom">
			<div class="container">
				<div class="footer-bottom-content">
					<div class="copyright">
						<p>{{ settings.copyright }}</p>
					</div>
					<div class="payment-card">
						<ul class="global-list">
							<li><img :src="getUrl('public/images/bkash.webp')" alt="payment_method_banner"
									class="img-fluid footer-payment-icon" width="50px" />
							</li>
							<li><img :src="getUrl('public/images/nagad.webp')" alt="payment_method_banner"
									class="img-fluid footer-payment-icon" width="50px" />
							</li>
							<li v-if="settings.visa_pay_banner"><img :src="getUrl('public/images/payment-method/visa.svg')"
									alt="visa_pay_banner" class="img-fluid footer-payment-icon" /></li>
							<li v-if="settings.master_card_pay_banner"><img
									:src="getUrl('public/images/payment-method/master-card.svg')"
									alt="master_card_pay_banner" class="img-fluid footer-payment-icon" /></li>
							<li v-if="settings.american_express_pay_banner"><img
									:src="getUrl('public/images/payment-method/american-express.svg')"
									alt="american_express_pay_banner" class="img-fluid footer-payment-icon" /> </li>
							<li v-if="settings.paypal_payment_banner"><img
									:src="getUrl('public/images/payment-method/paypal.svg')" alt="paypal_payment_banner"
									class="img-fluid footer-payment-icon" /></li>
							<li v-if="settings.apple_pay_banner"><img
									:src="getUrl('public/images/payment-method/apple-pay.svg')" alt="apple_pay_banner"
									class="img-fluid footer-payment-icon" /></li>
							<li v-if="settings.amazon_pay_banner"><img
									:src="getUrl('public/images/payment-method/amazon-pay.svg')" alt="amazon_pay_banner"
									class="img-fluid footer-payment-icon" /></li>
							<li v-if="settings.after_pay_banner"><img
									:src="getUrl('public/images/payment-method/after-pay.svg')" alt="after_pay_banner"
									class="img-fluid footer-payment-icon" /></li>

						</ul>
					</div>
				</div>
			</div><!-- /.container -->
		</div><!-- /.footer-bottom -->

		<div class="mb-bottom"></div>

		<div class="yoori--cookies" v-if="checkGDPR() && gdpr">
			<div class="cookie-content" v-html="settings.gdpr"> </div>
			<div class="cookie-btn">
				<button type="button" @click="setGDPR">{{ lang.accept_all }}</button>
			</div>
		</div>
		<div class="btnTOP"><span class="icon mdi mdi-name mdi-chevron-up"></span></div>
		<chat_system v-if="addons.includes('chat_system')"></chat_system>
	</footer><!-- /.footer-section -->
</template>

<script>
import chat_system from "../pages/addons/chat_system";

export default {
	name: "bottom",
	components: {
		chat_system
	},

	data() {
		return {
			gdpr: true
		};
	},
	mounted() {
		const scrollToTop = document.querySelector(".btnTOP");
		const showBTN = function () {
			if (window.scrollY > 300) {
				scrollToTop.style.opacity = 1;
				scrollToTop.style.transform = 'translateY(0px)';

			} else {
				scrollToTop.style.opacity = 0;
				scrollToTop.style.transform = 'translateY(100px)';
			}
		};
		document.addEventListener("scroll", () => {
			showBTN();
		});
		scrollToTop.addEventListener("click", function () {
			window.scroll({ top: 0, behavior: 'smooth' });
		});

		var width = 0;
		window.onresize = () => {
			width = window.innerWidth > 0 ? window.innerWidth : screen.width;
			let selector = $(".sg-seller-product .sg-category-content.sg-filter");
			let feature_selector = $(".sg-feature-shop .sg-filter");
			if (width < 575) {
				if (selector.hasClass("list-view-tab")) {
					selector.removeClass("list-view-tab");
				}
				if (feature_selector.hasClass("list-view-tab")) {
					feature_selector.removeClass("list-view-tab");
				}
			} else {
				if (selector.hasClass("list_restore")) {
					selector.addClass("list-view-tab");
				}
				feature_selector.addClass("list-view-tab");
			}
		}
	},
	computed: {
		usefulLinks() {
			return this.settings.useful_links;
		},
		footerMenu() {
			return this.settings.footer_menu;
		},
	},
	methods: {
		checkGDPR() {
			return !localStorage.getItem("gdpr") && this.settings.gdpr_enable == 1;
		},
		setGDPR() {
			this.gdpr = false;
			return localStorage.setItem("gdpr", "1");
		},
		classObj() {
			return [this.settings.seller_system == 1 && !this.authUser ? "col-lg-2" : "col-lg-3"];
		}
	},
};
</script>
