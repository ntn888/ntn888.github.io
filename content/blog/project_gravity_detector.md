+++
title = "Project: Gravity Detector"
date = 2023-05-06 00:27:00
draft = false

[taxonomies]
categories = ["nrf"]
tags = ["nRF52", "accelerometer", "sensors"]

[extra]
lang = "en"
toc = true
comment = false
copy = true
math = false
mermaid = false
outdate_alert = false
outdate_alert_days = 120
display_tags = true
truncate_summary = false
+++

Here we will hookup a MPU6050 accelerometer/gyro sensor to our micro:bit to detect the orientation of the board.

The microbit comes with an onboard accelerometer; but for the purpose of learning the procedure we will attach an external sensor to the board. The microbit also conveniently features a 5x5 LED Matrix, which we will use as a display to indicate direction. Basically we will imitate the effect of a dot on the display to flow towards the direction of gravity.

Given this is our first proper project with Zephyr, this will serve as an exercise of project bring-up and its structural nature.

---

We begin by cloning (or copying) the basic `hello world` project. Which will be our minimal starting point.

Hardware wise we attach the microbit to the breakout board and then attach it and the sensor to the breadboard. Then simply wire as follows:
![mpu6050 wiring diagram](/img/mpu6050conn.png)

We will communicate with the sensor via I^2^C bus. The actual driver file for the MPU6050 is already provided with the Zephyr sources! But we will need to provide an overlay file to reflect our custom wiring (which is the main exercise of using an external sensor). And remember to activate the modules in `prj.conf`.

`<zephyr_root>/applications/mpu6050/boards/bbc_microbit_v2.overlay`:
```

&pinctrl {
	i2c1_default: i2c1_default {
		group1 {
			psels = <NRF_PSEL(TWIM_SDA, 1, 0)>,
				<NRF_PSEL(TWIM_SCL, 0, 26)>;
		};
	};

	i2c1_sleep: i2c1_sleep {
		group1 {
			psels = <NRF_PSEL(TWIM_SDA, 1, 0)>,
				<NRF_PSEL(TWIM_SCL, 0, 26)>;
			low-power-enable;
		};
	};
};


&i2c1 {
	compatible = "nordic,nrf-twim";
	status = "okay";
	clock-frequency = <I2C_BITRATE_FAST>;

	pinctrl-0 = <&i2c1_default>;
	pinctrl-1 = <&i2c1_sleep>;
	pinctrl-names = "default", "sleep";
	/*sda-pin = <34>;
	scl-pin = <35>;*/
	mpu6050@68 {
		compatible = "invensense,mpu6050";
		reg = <0x68>;
		status = "okay";
		int-gpios = <&gpio0 2 GPIO_ACTIVE_HIGH>;
	};
};
```
`prj.conf`:
```
CONFIG_I2C=y
CONFIG_SENSOR=y
CONFIG_MPU6050_TRIGGER_NONE=y
CONFIG_CBPRINTF_FP_SUPPORT=y

CONFIG_DISPLAY=y
```

The basic overall application structure is as follows:

```
.
├── boards
│   └── bbc_microbit_v2.overlay
├── CMakeLists.txt
├── prj.conf
└── src
    └── main.c

```

## Communication with sensor

In our first iteration we will achieve basic interaction with the sensor module.

Luckily there is a sample application in the Zephyr: `<zephyr_root>/zephyr/samples/sensor/mpu6050/` directory from which we will copy the main.c contents:

```c
#include <zephyr/kernel.h>
#include <zephyr/device.h>
#include <zephyr/drivers/sensor.h>
#include <stdio.h>

static const char *now_str(void)
{
	static char buf[16]; /* ...HH:MM:SS.MMM */
	uint32_t now = k_uptime_get_32();
	unsigned int ms = now % MSEC_PER_SEC;
	unsigned int s;
	unsigned int min;
	unsigned int h;

	now /= MSEC_PER_SEC;
	s = now % 60U;
	now /= 60U;
	min = now % 60U;
	now /= 60U;
	h = now;

	snprintf(buf, sizeof(buf), "%u:%02u:%02u.%03u",
		 h, min, s, ms);
	return buf;
}

static int process_mpu6050(const struct device *dev)
{
	struct sensor_value temperature;
	struct sensor_value accel[3];
	struct sensor_value gyro[3];
	int rc = sensor_sample_fetch(dev);

	if (rc == 0) {
		rc = sensor_channel_get(dev, SENSOR_CHAN_ACCEL_XYZ,
					accel);
	}
	if (rc == 0) {
		rc = sensor_channel_get(dev, SENSOR_CHAN_GYRO_XYZ,
					gyro);
	}
	if (rc == 0) {
		rc = sensor_channel_get(dev, SENSOR_CHAN_AMBIENT_TEMP,
					&temperature);
	}
	if (rc == 0) {
		printf("[%s]:%g Cel\n"
		       "  accel %f %f %f m/s/s\n"
		       "  gyro  %f %f %f rad/s\n",
		       now_str(),
		       sensor_value_to_double(&temperature),
		       sensor_value_to_double(&accel[0]),
		       sensor_value_to_double(&accel[1]),
		       sensor_value_to_double(&accel[2]),
		       sensor_value_to_double(&gyro[0]),
		       sensor_value_to_double(&gyro[1]),
		       sensor_value_to_double(&gyro[2]));
	} else {
		printf("sample fetch/get failed: %d\n", rc);
	}

	return rc;
}

#ifdef CONFIG_MPU6050_TRIGGER
static struct sensor_trigger trigger;

static void handle_mpu6050_drdy(const struct device *dev,
				const struct sensor_trigger *trig)
{
	int rc = process_mpu6050(dev);

	if (rc != 0) {
		printf("cancelling trigger due to failure: %d\n", rc);
		(void)sensor_trigger_set(dev, trig, NULL);
		return;
	}
}
#endif /* CONFIG_MPU6050_TRIGGER */

int main(void)
{
	const struct device *const mpu6050 = DEVICE_DT_GET_ONE(invensense_mpu6050);

	if (!device_is_ready(mpu6050)) {
		printf("Device %s is not ready\n", mpu6050->name);
		return 0;
	}

#ifdef CONFIG_MPU6050_TRIGGER
	trigger = (struct sensor_trigger) {
		.type = SENSOR_TRIG_DATA_READY,
		.chan = SENSOR_CHAN_ALL,
	};
	if (sensor_trigger_set(mpu6050, &trigger,
			       handle_mpu6050_drdy) < 0) {
		printf("Cannot configure trigger\n");
		return 0;
	}
	printk("Configured for triggered sampling.\n");
#endif

	while (!IS_ENABLED(CONFIG_MPU6050_TRIGGER)) {
		int rc = process_mpu6050(mpu6050);

		if (rc != 0) {
			break;
		}
		k_sleep(K_SECONDS(2));
	}

	/* triggered runs with its own thread after exit */
	return 0;
}
```

The output on the Zephyr serial console:
```
*** Booting Zephyr OS build zephyr-v3.3.0-3014-ge59e65dc75e4 ***
[0:00:00.008]:19.9182 Cel
  accel 0.351947 -0.275334 10.371681 m/s/s
  gyro  -0.021583 -0.014655 -0.006794 rad/s
[0:00:02.019]:19.8712 Cel
  accel 0.272938 -0.150835 10.292672 m/s/s
  gyro  -0.019584 -0.009592 -0.012124 rad/s
```

## Output via LED Matrix

Now that we have confirmed successful communication with the module, we will strip off code related to console logging. And introduce the following function:
```c
void update_mydisplay(double y, double z, const struct device *dev)
{
	// makeup buf here
/*	buf[0] = PIXEL_MASK(1, 0, 1, 0, 1);
	buf[1] = PIXEL_MASK(1, 1, 0, 0, 1);
	buf[2] = PIXEL_MASK(1, 0, 1, 0, 1);
	buf[3] = PIXEL_MASK(1, 0, 0, 1, 1);
	buf[4] = PIXEL_MASK(1, 0, 1, 0, 1);
*/
	uint8_t tbuf[5][5];
	int i, j, ii, jj, ret;

	for (i = 0; i < 5; i++)
		for (j = 0; j < 5; j++)
			tbuf[i][j] = 0;

	// work the dot here
	jj = 0 - y/10*5 + 2;
	ii = z/10*5 + 2;

	// assign ceiling values to trap the dot into display!
	if (ii > 4)
		ii = 4;
	if (jj > 4)
		jj = 4;
	if (ii < 0)
		ii = 0;
	if (jj < 0)
		jj = 0;

	tbuf[ii][jj] = 1;
	for (i = 0; i < 5; i++)
		buf[i] = 0;
	
	for (i = 0; i < 5; i++)
		for (j = 0; j < 5; j++)
		{
			buf[i] |= tbuf[i][j];
			if (j < 4)
				buf[i] <<= 1;
		}

	ret = display_set_brightness(dev, 0x7F);
	if (ret < 0) {
		printk("display_set_brightness failed: %u/%d\n",
			__LINE__, ret);
	}
	ret = display_write(dev, 0, 0, &buf_desc, buf);
	if (ret < 0) {
		printk("display_write failed: %u/%d\n",
			__LINE__, ret);
	}

	ret = display_blanking_off(dev);
	if (ret < 0) {
		printk("display_blanking_off failed: %u/%d\n",
			__LINE__, ret);
	}

}
```

In the code above, we use an intermediate 2d array to represent the LED Matrix to flexibly refer to our indicating dot. This *dot* will move towards the direction of flowing gravity. Now our sensor is 3-dimensional but we only consider 2 axes for the 2d array. By means of trial-and-error, this works out to be the x and z axis of the sensor, which is passed to this function.

Since our final LED Matrix data is single dimensional 5bit array; we need to populate this data by means of our temporary 2d array. We do this by bitwise shifting to the left while cycling through each row and copy it through.

And we need to periodically call this function after processing sensor data. Here's the updated main.c:

```c
#include <zephyr/kernel.h>
#include <zephyr/device.h>
#include <zephyr/drivers/display.h>
#include <zephyr/drivers/sensor.h>
#include <stdio.h>

#define PIXEL_BIT(idx, val)  (val ? BIT(idx) : 0)
#define PIXEL_MASK(...)      (FOR_EACH_IDX(PIXEL_BIT, (|), __VA_ARGS__))

static struct display_capabilities caps;
static uint8_t buf[5];
static const struct display_buffer_descriptor buf_desc = {
	.buf_size = sizeof(buf),
	.width    = 5,
	.height   = 5,
	.pitch    = 8,
};

void update_mydisplay(double y, double z, const struct device *dev)
{
	// makeup buf here
/*	buf[0] = PIXEL_MASK(1, 0, 1, 0, 1);
	buf[1] = PIXEL_MASK(1, 1, 0, 0, 1);
	buf[2] = PIXEL_MASK(1, 0, 1, 0, 1);
	buf[3] = PIXEL_MASK(1, 0, 0, 1, 1);
	buf[4] = PIXEL_MASK(1, 0, 1, 0, 1);
*/
	uint8_t tbuf[5][5];
	int i, j, ii, jj, ret;

	for (i = 0; i < 5; i++)
		for (j = 0; j < 5; j++)
			tbuf[i][j] = 0;

	// work the dot here
	jj = 0 - y/10*5 + 2;
	ii = z/10*5 + 2;

	// assign ceiling values to trap the dot into display!
	if (ii > 4)
		ii = 4;
	if (jj > 4)
		jj = 4;
	if (ii < 0)
		ii = 0;
	if (jj < 0)
		jj = 0;

	tbuf[ii][jj] = 1;
	for (i = 0; i < 5; i++)
		buf[i] = 0;
	
	for (i = 0; i < 5; i++)
		for (j = 0; j < 5; j++)
		{
			buf[i] |= tbuf[i][j];
			if (j < 4)
				buf[i] <<= 1;
		}

	ret = display_set_brightness(dev, 0x7F);
	if (ret < 0) {
		printk("display_set_brightness failed: %u/%d\n",
			__LINE__, ret);
	}
	ret = display_write(dev, 0, 0, &buf_desc, buf);
	if (ret < 0) {
		printk("display_write failed: %u/%d\n",
			__LINE__, ret);
	}

	ret = display_blanking_off(dev);
	if (ret < 0) {
		printk("display_blanking_off failed: %u/%d\n",
			__LINE__, ret);
	}

}

static const char *now_str(void)
{
	static char buf[16]; /* ...HH:MM:SS.MMM */
	uint32_t now = k_uptime_get_32();
	unsigned int ms = now % MSEC_PER_SEC;
	unsigned int s;
	unsigned int min;
	unsigned int h;

	now /= MSEC_PER_SEC;
	s = now % 60U;
	now /= 60U;
	min = now % 60U;
	now /= 60U;
	h = now;

	snprintf(buf, sizeof(buf), "%u:%02u:%02u.%03u",
		 h, min, s, ms);
	return buf;
}

static int process_mpu6050(const struct device *dev, const struct device *ddev)
{
	struct sensor_value temperature;
	struct sensor_value accel[3];
	struct sensor_value gyro[3];
	int rc = sensor_sample_fetch(dev);

	if (rc == 0) {
		rc = sensor_channel_get(dev, SENSOR_CHAN_ACCEL_XYZ,
					accel);
	}
	if (rc == 0) {
		rc = sensor_channel_get(dev, SENSOR_CHAN_GYRO_XYZ,
					gyro);
	}
	if (rc == 0) {
		rc = sensor_channel_get(dev, SENSOR_CHAN_AMBIENT_TEMP,
					&temperature);
	}
	if (rc == 0) {
/*		printf("[%s]:%g Cel\n"
		       "  accel %f %f %f m/s/s\n"
		       "  gyro  %f %f %f rad/s\n",
		       now_str(),
		       sensor_value_to_double(&temperature),
		       sensor_value_to_double(&accel[0]),
		       sensor_value_to_double(&accel[1]),
		       sensor_value_to_double(&accel[2]),
		       sensor_value_to_double(&gyro[0]),
		       sensor_value_to_double(&gyro[1]),
		       sensor_value_to_double(&gyro[2]));*/
	} else {
		printf("sample fetch/get failed: %d\n", rc);
	}

	update_mydisplay(sensor_value_to_double(&accel[0]), sensor_value_to_double(&accel[2]), ddev);

	return rc;
}


int main(void)
{
	const struct device *const mpu6050 = DEVICE_DT_GET_ONE(invensense_mpu6050);

	if (!device_is_ready(mpu6050)) {
		printf("Device %s is not ready\n", mpu6050->name);
		return 0;
	}

	int ret;
	const struct device *const dev = DEVICE_DT_GET_ONE(nordic_nrf_led_matrix);

	if (!dev) {
		printk("Display device not ready\n");
		return 0;
	}

	display_get_capabilities(dev, &caps);
	if (!(caps.supported_pixel_formats & PIXEL_FORMAT_MONO01)) {
		printk("Expected pixel format not supported\n");
		return 0;
	}

	ret = display_set_pixel_format(dev, PIXEL_FORMAT_MONO01);
	if (ret < 0) {
		printk("display_set_pixel_format failed: %u/%d\n",
			__LINE__, ret);
	}

	while (!IS_ENABLED(CONFIG_MPU6050_TRIGGER)) {
		int rc = process_mpu6050(mpu6050, dev);

		if (rc != 0) {
			break;
		}
		k_msleep(100);
	}

	/* triggered runs with its own thread after exit */
	return 0;
}

```

![animation final effect](/img/accel_final_effect.gif)
