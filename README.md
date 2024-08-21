class FavoriteCardSlider {
 constructor(container) {
 this.container = container;
 this.favoriteCardsObject = {};
 this.initializeSliders();
 this.addEventListeners();
 }

 initializeSliders() {
 const favoriteCards = this.container.querySelectorAll(".object_card");

 favoriteCards.forEach((card, index) => {
 this.favoriteCardsObject[`slider${index}`] = {
 sliderIndex: 0,
 sliderChildCount: this.getSliderChildCount(card),
 sliderContainer: this.getSliderContainer(card),
 };
 });
 }

 getSliderChildCount(cardNode) {
 const sliderList = cardNode.querySelectorAll(".car_images ul li");
 return sliderList.length;
 }

 getSliderContainer(cardNode) {
 return cardNode.querySelector(".car_images ul");
 }

 addEventListeners() {
 this.container.querySelectorAll('.slider-button').forEach(button => {
 button.addEventListener('click', (event) => {
 const increment = button.classList.contains('left') ? -1 : 1;
 this.navigateSlider(increment, event.currentTarget);
 });
 });
 }

 navigateSlider(indexIncrement, currentElement) {
 const sliderCard = currentElement.closest('.object_card');
 const sliderId = sliderCard.getAttribute("id");
 const sliderIndex = parseInt(sliderId.match(/\d+/)[0]);
 this.updateSlider(indexIncrement, sliderIndex);
 }

 updateSlider(indexIncrement, sliderIndex) {
 const sliderData = this.favoriteCardsObject[`slider${sliderIndex}`];
 const sliderContainer = sliderData.sliderContainer;
 const sliderItems = sliderContainer.querySelectorAll("li");

 const totalSlides = Math.floor(((sliderItems.length * sliderItems[0].offsetWidth) - sliderContainer.offsetWidth) / sliderItems[0].offsetWidth) + 1;
 sliderData.sliderIndex += indexIncrement;

 if (sliderData.sliderIndex > totalSlides) {
 sliderData.sliderIndex = 0;
 } else if (sliderData.sliderIndex < 0) {
 sliderData.sliderIndex = totalSlides;
 }

 const cardGap = (window.innerWidth > 580) ? 16 : 8;
 sliderContainer.scrollLeft = sliderData.sliderIndex * (sliderItems[0].offsetWidth + cardGap);
 }
}
