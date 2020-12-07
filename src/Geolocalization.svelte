<script>
  if ('geolocation' in navigator) {
    navigator.geolocation.getCurrentPosition(position => {
      const map = L.map('map').setView([position.coords.latitude, position.coords.longitude], 13)
      const tile = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', 
                              { attribution: '& copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors' }).addTo(map) // required
      const icon = L.icon({ 'urlToIcon.svg', iconSize: [30, 30], iconAnchor: [15, 30] });
      const marker = L.marker([position.coords.latitude, position.coords.longitude], { icon }).addTo(map).bindPopup('Localization<br>information') //.openPopup()
    })
  } else {
    document.querySelector('#map').textContent = 'geolocation not available'
  }
</script>

<div id="map" style="width: 100%; height: 90vh; opacity: .6; filter: grayscale(90%)"></div>