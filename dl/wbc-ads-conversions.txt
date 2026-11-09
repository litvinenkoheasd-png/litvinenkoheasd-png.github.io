<?php
/**
 * Plugin Name: WBC Google Ads conversions
 * Description: Counts clicks on WhatsApp links and phone numbers as Google Ads conversions (Astana + Shymkent accounts).
 * Version: 1.0
 */
if (!defined('ABSPATH')) { exit; }

add_action('wp_footer', function () {
    ?>
<script>
(function () {
  window.dataLayer = window.dataLayer || [];
  if (typeof window.gtag !== 'function') { window.gtag = function () { window.dataLayer.push(arguments); }; }
  window.gtag('config', 'AW-17860424692');
  var ACC = {
    shymkent: { wa: 'AW-17860424692/p7iyCNKNn7UdEPTnwcRC', tel: 'AW-17860424692/FG1SCPSRn7UdEPTnwcRC' },
    astana:   { wa: 'AW-17602826785/TqadCLaZmLUdEKGk18lB', tel: 'AW-17602826785/J0vuCLLmn7UdEKGk18lB' }
  };
  function account(href) {
    var h = (href || '').replace(/\D/g, '');
    if (h.indexOf('77078928614') !== -1 || /shymkent|shym/i.test(location.pathname)) { return ACC.shymkent; }
    return ACC.astana;
  }
  document.addEventListener('click', function (e) {
    var a = e.target && e.target.closest ? e.target.closest('a[href]') : null;
    if (!a) { return; }
    var href = a.getAttribute('href') || '';
    var kind = /wa\.me|api\.whatsapp\.com|whatsapp:/i.test(href) ? 'wa' : (/^tel:/i.test(href) ? 'tel' : null);
    if (!kind) { return; }
    window.gtag('event', 'conversion', { send_to: account(href)[kind], transport_type: 'beacon' });
  }, true);
})();
</script>
    <?php
}, 100);
