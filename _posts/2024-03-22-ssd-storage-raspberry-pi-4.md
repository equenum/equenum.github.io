---
title: Choosing SSD storage for a Raspberry Pi 4 micro server
date: 2024-03-22 13:00:00 +0400
categories: [homelab]
tags: [raspberry-pi, hardware]
image:
  path: assets/img/posts/20240322/rpi4_ssd_thumbnail.webp
  lqip: data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAMCAYAAABiDJ37AAAiBHpUWHRSYXcgcHJvZmlsZSB0eXBlIGV4aWYAAHja7Z1pkuM6sqX/cxVvCcQMLAejWe+gl9/fASmFFBk53Vt1q6xfRppSIVAEAR+OH3eAjGP+3/+zjv/5n/8xIZ/x8CHlWGI8+fHFF1v5JZ/
---

Welcome to this blog on how I chose SSD drives for my [Raspberry Pi 4 Homelab micro server]({% post_url 2024-03-02-my-homelab-server-setup-v1.0 %}). This process turned out to be more tricky than originally expected, including finding the information to base my research on. Hope this blog helps you make the choice quicker and hassle-free or just serves as an interesting read.

Please note that at this time, I mostly concentrate on power efficiency and don't get into read / write performance. However, I believe that some of the resources I mention might help compare RPi-compatible SSDs by those metrics as well.

## Goals and restrictions

When researching for my future Homelab server hardware configuration last year, I decided to go with 2 SSD drives: 120 and 240 GB. There were multiple reasons behind it, the main being to not use the same drive as a boot device and a drive for my network file-sharing shenanigans. By having 2 drives I can safely work with my Linux distro (experiment / reinstall) on one without affecting the other which serves as a very basic NAS.

At the same time, I did not want to power the drives externally, so that the setup stays as simple as possible. This meant that I had to power them through the Pi's USB ports. This, of course, comes with some challenges. Let's look into them.

### RPi 4 power output limitations

RPi 4 is equipped with 4 USB ports (2 USB 3.0, 2 USB 2.0) and is [rated for 1200 mA maximum power output](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#maximum-power-output) total across all ports while being powered with a 5.1 V 3.0 A power supply. This means that if I want to power both the SSDs of my RPi 4 they better not consume more than 6000 mW total peak (USB ports are 5.0 V). This is, of course, before accounting for a case fan or any possible HATs. I decided to try and aim for half as much (3000 mW) to not only stay on the safer side in terms of possible undervoltage issues but also when it comes to leaving room for any possible additional hardware I might want to use / install in the future.

## Research & Comparison

After looking through numerous articles and Reddit posts, I was able to approximate several SSD series that seem to be the most popular community choices:

- [Kingston A400](https://www.kingston.com/en/ssd/a400-solid-state-drive?partnum=sa400s37%2F120g).
- [Crucial BX500](https://www.crucial.com/ssd/bx500/ct240bx500ssd1).
- [WD Green](https://www.westerndigital.com/en-au/products/internal-drives/wd-green-sata-2-5-ssd?sku=WDS120G2G0A).

Even though, all of those are very popular in my location as well, unfortunately, some of the models were not available locally in 120 and / or 240 GB versions. For that reason, I decided to also look into other less popular local options. There was not a lot of them, though. Basically, just one - [Patriot Burst Elite](https://www.patriotmemory.com/products/burst-elite-sata-iii-2-5-ssd). There were both 120 and 240 GB versions available, so I decided to also add this series to my research list.

### Official specifications

I started by looking up official datasheets and specs for all of the series, mostly concentrating on power consumption and less on read / write performance since my application does not require crazy speeds and durability is what I am after. Here is the table (official manufacturer specs only) I ended up with along with the links to the related product pages for 120 GB versions:

| Brand / Series | Part Number                                                                                                        | Seq. Read, MB/s | Seq. Write, MB/s | Max. Power (read), mW | Max. Power (write), mW |
| :------------- | :----------------------------------------------------------------------------------------------------------------- | :-------------- | :--------------- | :-------------------- | ---------------------- |
| WD Green       | [WDS120G2G0A](https://www.westerndigital.com/en-au/products/internal-drives/wd-green-sata-2-5-ssd?sku=WDS120G2G0A) | 545             | 80\*             | 2200                  | 2200                   |
| Patriot Burst  | [PBE120GS25SSDR](https://www.patriotmemory.com/products/burst-elite-sata-iii-2-5-ssd)                              | 450             | 320              | N/A                   | N/A                    |
| Kingston A400  | [SA400S37/120G](https://www.kingston.com/en/ssd/a400-solid-state-drive?partnum=sa400s37%2F120g)                    | 500             | 320              | 642                   | 1535                   |

<sub>NOTE: Crucial BX500 series does not offer a 120 GB version: it goes 240 GB and up.</sub><br />

And the same for 240 GB:

| Brand / Series | Part Number                                                                                                        | Seq. Read, MB/s | Seq. Write, MB/s | Max. Power (read), mW | Max. Power (write), mW |
| :------------- | :----------------------------------------------------------------------------------------------------------------- | :-------------- | :--------------- | :-------------------- | ---------------------- |
| WD Green       | [WDS240G3G0A](https://www.westerndigital.com/en-au/products/internal-drives/wd-green-sata-2-5-ssd?sku=WDS240G3G0A) | 545             | N/A\*            | 2800                  | 2800                   |
| Patriot Burst  | [PBE240GS25SSDR](https://www.patriotmemory.com/products/burst-elite-sata-iii-2-5-ssd)                              | 450             | 320              | N/A                   | N/A                    |
| Crucial BX500  | [CT240BX500SSD1](https://www.crucial.com/ssd/bx500/ct240bx500ssd1)                                                 | 540             | 500              | N/A                   | N/A                    |
| Kingston A400  | [SA400S37/240GB](https://www.kingston.com/en/ssd/a400-solid-state-drive?partnum=sa400s37%2F240g)                   | 500             | 350              | 642                   | 1535                   |

<sub>\* Sequential Write performance specs for both WD Green SSDs were not specified in the official documentation or conflicting / questionable values were provided. For example, WD's website product description states that the Seq. Write performance for WDS120G2G0A is 80 MB/s, when numerous other sources, mention values between 465 and 600. When it comes to WDS240G3G0A, the Seq. Write performance table cell is just blank.</sub><br />
<sub>NOTE: Seq. Read - Sequential Read Performance (up to), MB/s; Seq. Write - Sequential Write Performance (up to), MB/s; Max. Power - Maximum Power Consumption Operating, mW.</sub><br />

Unfortunately, half of the manufacturer datasheets and website descriptions don't contain power consumption specifications. It seems to be a norm for consumer-grade SSD documentation for some reason. But it is fine since I intended to mostly base my research on community-driven power consumption benchmarks anyway. The idea was to find a single resource where I could get power consumption benchmarks for all the SSDs in question. In theory, this might help ensure that even if the testing methodology is not the best the data should still allow for adequately comparing the drives since it is at least, hopefully, consistent for all the tests.

### Power consumption benchmarks

In my research, I found [tuxad.com](https://www.tuxad.com/blog/archives/) - a tech blog / IT Service website by Frank W. Bergmann. It proved to be very useful in my research since there I was able to find a series of SSD power consumption and write / read performance benchmarks that cover all the SSDs in question, except for, unfortunately, WDS240G3G0A. I was also not able to find any other community-driven benchmarks for this model.

When it comes to other benchmarks it seems like most of them date back to 2018 - 2019. Sure, it is possible that the manufacturers have updated some of the internal components that go into their drives since then, and the tests might not be all that relevant nowadays. At the same time, this is the best I could find according to my criteria since most of the other benchmark articles seem to be either heavily write / read performance-oriented and just recite the official specs when it comes to power consumption. Please reach out if you know where to find better benchmarks. I will try my best to update this blog entry with more recent data if available.

Here you can find a [description of the test environment](https://www.tuxad.com/blog/archives/2018/04/04/power_measuring_adata_su800/index.html) used for testing. I have put together tables and charts to help better visualize the benchmark results. The tables also include links to the related benchmark posts. Let's start with 120 GB SSDs:

| Brand / Series | Part Number    | Max. Power (read), mW | Max. Power (write), mW | Benchmark                                                                                                     |
| :------------- | :------------- | :-------------------- | :--------------------- | :------------------------------------------------------------------------------------------------------------ |
| WD Green       | WDS120G2G0A    | 960                   | 1400                   | [tuxad.com](https://www.tuxad.com/blog/archives/2018/04/09/power_measuring_wd_green_pc_ssd_120_gb/index.html) |
| Patriot Burst  | PBE120GS25SSDR | 540                   | 540                    | [tuxad.com](https://www.tuxad.com/blog/archives/2019/01/06/power_measuring_patriot_burst_120_gb/index.html)   |
| Kingston A400  | SA400S37/120G  | 510                   | 1050                   | [tuxad.com](https://www.tuxad.com/blog/archives/2018/10/03/power_measuring_kingston_a400_120_gb/index.html)   |

![Desktop View](/assets/img/posts/20240322/120_chart_dark.webp){: width="601" height="339" }
_120 GB drive series performance testing results_

And the same for 240 GB:

| Brand / Series | Part Number    | Max. Power (read), mW | Max. Power (write), mW | Benchmark                                                                                                                             |
| :------------- | :------------- | :-------------------- | :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------ |
| WD Green       | WDS240G3G0A    | N/A                   | N/A                    | N/A                                                                                                                                   |
| Patriot Burst  | PBE240GS25SSDR | 520                   | 530                    | [tuxad.com](https://www.tuxad.com/blog/archives/2019/12/16/power_measuring_patriot_burst_240_and_verbatim_vi500_s3_240480/index.html) |
| Crucial BX500  | CT240BX500SSD1 | 600                   | 3300                   | [tuxad.com](https://www.tuxad.com/blog/archives/2020/02/02/power_measuring_crucial_bx500_240/index.html)                              |
| Kingston A400  | SA400S37/240GB | 520                   | 1560                   | [tuxad.com](https://www.tuxad.com/blog/archives/2019/02/25/power_measuring_kingston_a400_240_gb/index.html)                           |

![Desktop View](/assets/img/posts/20240322/240_chart_dark.webp){: width="699" height="339" }
_240 GB drive series performance testing results_

Please keep in mind that as noted on the 240 GB chart I extrapolated the power consumption data for WDS240G3G0A based on WDS120G2G0A results and official specs. But only for the chart visualization. Sure, probably unreliable, but hopefully somewhere in the same ballpark with the real values.

### Community trends

As part of my research I also went through [pibenchmarks.com](https://pibenchmarks.com/) - the biggest community-driven [Raspberry Pi-oriented benchmarking](https://github.com/TheRemote/PiBenchmarks?tab=readme-ov-file) platform by James A. Chambers. You can read more about the testing methodology [here](https://jamesachambers.com/raspberry-pi-storage-benchmarks-2019-benchmarking-script/).

It is a great place not to just see how different storage media perform on RPis, but also to get a feel for the community trends in terms of hardware popularity and overall performance ranking. Again, I did not get into write / read performance too deeply, since I mostly wanted to know which SSDs seemed to work out well for other RPi users. If you decide to explore the test results closer, please make sure to be careful when interpreting the results, since the tests for the same drives are performed on different RPi software (different OS Linux distros, etc.) and hardware configurations (SATA to USB adaptors, RAM size, CPU frequency due to overclocking, etc.). Make sure to find test results that are as close to your setup as possible.

Here are the SSD series statistics based on the huge amounts of community-driven benchmarks (as of 22 March 2024):

| Brand / Series                                                                      | Popularity (Overall), # / 1539 | Rank (Overall), # / 1539 | Rank (Type), # / 875 | Rank (Class), # / 363 |
| :---------------------------------------------------------------------------------- | :----------------------------- | :----------------------- | :------------------- | :-------------------- |
| [ WD Green ](https://pibenchmarks.com/brand/Western_Digital_Green/)                 | 24                             | 776                      | 740                  | 279                   |
| [ Patriot Burst Elite ](https://pibenchmarks.com/brand/Patriot_Burst_Elite_Series/) | 160                            | 598                      | 576                  | 186                   |
| [ Kingston A400S ](https://pibenchmarks.com/brand/Kingston_A400S_Series/)           | **1**                          | 516                      | 496                  | 140                   |
| [ **Crucial BX500** ](https://pibenchmarks.com/brand/Crucial_BX500_Series/)         | 4                              | **487**                  | **467**              | **125**               |

Notice, that the popularity and performance rankings are based on storage types (SSD, HDD, SD, possibly others) and classes (SSD (M.2 NVMe), SSD (mSATA), SSD (Portable), SSD (2.5" SATA), HDD (3.5" SATA), USB Flash, possibly others). This means that those metrics compare USB drives with M.2 NVMe SSDs and so on. Please make sure to keep this in mind as well when interpreting the test results.

## Conclusion

Considering the research results, I went with Patriot Burst PBE120GS25SSDR + Patriot Burst PBE240GS25SSDR with the total theoretical combined power consumption of 1060 mW read and about the same for write operations. Those drives are obviously the weakest out of the bunch in terms of write / read performance, but seem to be by far the most power-efficient (which makes sense) in some cases beating the other drives by upwards of around 5 times. Since my application is not speed-critical at all it seemed to make more sense to go with Patriot Burst. Those drives were also noticeably cheaper locally than the other ones. At this point, I've been using them for around 3 months running my micro server 24/7. No issues so far in terms of drive operation and undervaltage warnings.

If you are not looking to power several drives of your RPi, it probably makes sense to look into Kingston A400S and Crucial BX500 series for better performance. WD Green seems to be quite power-hungry while not really getting even close to Kingston A400S and Crucial BX500 levels of performance. Maybe it is specific to RPis though.

Anyway, I hope you found this blog entry useful. Let your SSD choice be quick and painless, and RPi lightning fast and not undervolted!

## Note on SATA to USB 3.0 adapter cables

There is one thing worth mentioning about SATA cables. So far, it seems like the best compatible ones are based on the ASMedia ASM1153E/ASM225 chip. Those that are not might not work properly or at all. James A. Chambers keeps [a list of many known compatible (and not) storage adapters](https://jamesachambers.com/best-ssd-storage-adapters-for-raspberry-pi-4-400/) that you might find useful (scroll down to Full Storage Adapters Compatibility List).

## Useful URLs

- <https://www.raspberrypi.com/documentation/computers/raspberry-pi.html>
- <https://raspberrypi.stackexchange.com/questions/114239/pi-4-maximum-power-consumption>
- <https://jamesachambers.com/best-ssd-storage-adapters-for-raspberry-pi-4-400/>
- <https://pibenchmarks.com/fastest/>
- <https://www.jeffgeerling.com/blog/2023/using-pibenchmarkscom-sbc-disk-performance-testing>
- <https://jamesachambers.com/benchmark-storage-performance-on-linux/>
- <https://github.com/TheRemote/PiBenchmarks>

## Official datasheet PDFs

**Caution, PDFs!** Found on official manufacturer product pages, scanned for malware via **virustotal.com**.

- **WD Green:** <https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/product/internal-drives/wd-green-ssd/product-brief-wd-green-ssd.pdf>

- **Crucial BX500:** <https://content.crucial.com/content/dam/crucial/ssd-products/bx500/flyer/crucial-bx500-ssd-productflyer.pdf>

- **Kingston A400:** <https://www.kingston.com/datasheets/sa400_en.pdf>

- **Patriot Burst:** can be found on the official website. Did not include it here, since virustotal.com flagged it as suspicious on one of their 93 metrics.
