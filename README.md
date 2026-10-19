import React, {
  createContext,
  useContext,
  useState,
} from "react";

import {
  SafeAreaView,
  ScrollView,
  StyleSheet,
  Text,
  View,
  Image,
  Pressable,
  useWindowDimensions,
} from "react-native";

/* =====================================================
   ДАННЫЕ ПРИЛОЖЕНИЯ
===================================================== */

const categories = [
  "Достопримечательности",
  "Парки",
  "Рестораны",
];

const places = [
  {
    id: "1",
    name: "Айя-София",
    category: "Достопримечательности",
    description:
      "Айя-София — одна из самых известных достопримечательностей Стамбула. Историческое сооружение расположено в районе Султанахмет и является важной частью культурного наследия города.",
    address: "Sultan Ahmet, Fatih, Istanbul",
    rating: 4.9,
    image:
      "https://images.unsplash.com/photo-1541432901042-2d8bd64b4a9b?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "2",
    name: "Голубая мечеть",
    category: "Достопримечательности",
    description:
      "Голубая мечеть, также известная как мечеть Султанахмет, является одной из главных архитектурных достопримечательностей Стамбула.",
    address: "Sultan Ahmet, Fatih, Istanbul",
    rating: 4.8,
    image:
      "https://images.unsplash.com/photo-1609137144813-7d9921338f24?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "3",
    name: "Дворец Топкапы",
    category: "Достопримечательности",
    description:
      "Дворец Топкапы — исторический дворцовый комплекс Османской империи. Сегодня здесь находится музей.",
    address: "Cankurtaran, Fatih, Istanbul",
    rating: 4.7,
    image:
      "https://images.unsplash.com/photo-1524231757912-21f4fe3a7200?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "4",
    name: "Галатская башня",
    category: "Достопримечательности",
    description:
      "Галатская башня — одна из самых узнаваемых исторических построек Стамбула. С района башни открываются красивые виды на город.",
    address: "Bereketzade, Beyoğlu, Istanbul",
    rating: 4.7,
    image:
      "https://images.unsplash.com/photo-1524231757912-21f4fe3a7200?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "5",
    name: "Парк Эмирган",
    category: "Парки",
    description:
      "Парк Эмирган — большой зеленый парк на европейской стороне Стамбула. Он особенно известен своими тюльпанами.",
    address: "Emirgan, Sarıyer, Istanbul",
    rating: 4.8,
    image:
      "https://images.unsplash.com/photo-1490750967868-88aa4486c946?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "6",
    name: "Парк Гюльхане",
    category: "Парки",
    description:
      "Исторический парк рядом с дворцом Топкапы. Отличное место для спокойной прогулки в центре Стамбула.",
    address: "Cankurtaran, Fatih, Istanbul",
    rating: 4.7,
    image:
      "https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "7",
    name: "Парк Йылдыз",
    category: "Парки",
    description:
      "Большой городской парк района Бешикташ с лесными дорожками, зелеными зонами и историческими павильонами.",
    address: "Yıldız, Beşiktaş, Istanbul",
    rating: 4.6,
    image:
      "https://images.unsplash.com/photo-1473445361085-b9a07f55608b?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "8",
    name: "Бельградский лес",
    category: "Парки",
    description:
      "Большая лесная территория на европейской стороне Стамбула. Подходит для прогулок, бега и отдыха.",
    address: "Sarıyer, Istanbul",
    rating: 4.7,
    image:
      "https://images.unsplash.com/photo-1441974231531-c6227db76b6e?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "9",
    name: "Pandeli",
    category: "Рестораны",
    description:
      "Исторический ресторан возле Египетского базара, известный традиционной турецкой кухней.",
    address: "Eminönü, Fatih, Istanbul",
    rating: 4.6,
    image:
      "https://images.unsplash.com/photo-1547592180-85f173990554?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "10",
    name: "Karaköy Lokantası",
    category: "Рестораны",
    description:
      "Популярный ресторан в районе Каракёй с современным интерьером и блюдами традиционной турецкой кухни.",
    address: "Karaköy, Beyoğlu, Istanbul",
    rating: 4.7,
    image:
      "https://images.unsplash.com/photo-1552566626-52f8b828add9?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "11",
    name: "Balkan Lokantası",
    category: "Рестораны",
    description:
      "Ресторан с домашней турецкой кухней. Здесь можно попробовать традиционные блюда.",
    address: "Beyoğlu, Istanbul",
    rating: 4.5,
    image:
      "https://images.unsplash.com/photo-1515003197210-e0cd71810b5f?auto=format&fit=crop&w=1000&q=80",
  },

  {
    id: "12",
    name: "360 Istanbul",
    category: "Рестораны",
    description:
      "Ресторан с панорамными видами на Стамбул и блюдами современной турецкой кухни.",
    address: "İstiklal Caddesi, Beyoğlu, Istanbul",
    rating: 4.5,
    image:
      "https://images.unsplash.com/photo-1517248135467-4c7edcad34c4?auto=format&fit=crop&w=1000&q=80",
  },
];

/* =====================================================
   CONTEXT API
===================================================== */

const PlacesContext = createContext(null);

function PlacesProvider({ children }) {
  const [selectedCategory, setSelectedCategory] =
    useState(null);

  const [selectedPlace, setSelectedPlace] =
    useState(null);

  const selectCategory = (category) => {
    setSelectedCategory(category);
    setSelectedPlace(null);
  };

  const selectPlace = (place) => {
    setSelectedPlace(place);
  };

  return (
    <PlacesContext.Provider
      value={{
        places,
        categories,
        selectedCategory,
        selectedPlace,
        selectCategory,
        selectPlace,
      }}
    >
      {children}
    </PlacesContext.Provider>
  );
}

function usePlaces() {
  const context = useContext(PlacesContext);

  if (!context) {
    throw new Error(
      "usePlaces должен использоваться внутри PlacesProvider"
    );
  }

  return context;
}

/* =====================================================
   КАРТОЧКА КАТЕГОРИИ
===================================================== */

function CategoryCard({ title, onPress }) {
  let icon = "📍";

  if (title === "Достопримечательности") {
    icon = "🏛️";
  }

  if (title === "Парки") {
    icon = "🌳";
  }

  if (title === "Рестораны") {
    icon = "🍽️";
  }

  return (
    <Pressable
      onPress={onPress}
      style={({ pressed }) => [
        styles.categoryCard,
        pressed && styles.pressed,
      ]}
    >
      <View style={styles.categoryIcon}>
        <Text style={styles.categoryIconText}>
          {icon}
        </Text>
      </View>

      <View style={styles.categoryContent}>
        <Text style={styles.categoryTitle}>
          {title}
        </Text>

        <Text style={styles.categorySubtitle}>
          Смотреть места →
        </Text>
      </View>
    </Pressable>
  );
}

/* =====================================================
   КАРТОЧКА МЕСТА
===================================================== */

function PlaceCard({ place, onPress }) {
  const { width } = useWindowDimensions();

  const isTablet = width >= 768;

  return (
    <Pressable
      onPress={onPress}
      style={({ pressed }) => [
        styles.placeCard,
        isTablet && styles.placeCardTablet,
        pressed && styles.pressed,
      ]}
    >
      <Image
        source={{ uri: place.image }}
        style={[
          styles.placeImage,
          isTablet && styles.placeImageTablet,
        ]}
      />

      <View style={styles.placeContent}>
        <Text
          style={styles.placeName}
          numberOfLines={2}
        >
          {place.name}
        </Text>

        <Text
          style={styles.placeAddress}
          numberOfLines={2}
        >
          📍 {place.address}
        </Text>

        <View style={styles.ratingRow}>
          <Text style={styles.star}>★</Text>

          <Text style={styles.rating}>
            {place.rating}
          </Text>
        </View>
      </View>
    </Pressable>
  );
}

/* =====================================================
   ЭКРАН КАТЕГОРИЙ
===================================================== */

function CategoriesScreen({
  onOpenCategory,
  onOpenProfile,
}) {
  const { categories } = usePlaces();

  const { width } = useWindowDimensions();

  const isTablet = width >= 768;

  return (
    <SafeAreaView style={styles.screen}>
      <ScrollView
        contentContainerStyle={[
          styles.container,
          isTablet && styles.containerTablet,
        ]}
      >
        <View style={styles.header}>
          <Text style={styles.flag}>🇹🇷</Text>

          <Text style={styles.mainTitle}>
            Стамбул
          </Text>

          <Text style={styles.mainSubtitle}>
            Путеводитель по достопримечательностям,
            паркам и ресторанам города
          </Text>
        </View>

        <Text style={styles.sectionTitle}>
          Категории
        </Text>

        <View
          style={[
            styles.categoriesContainer,
            isTablet && styles.categoriesTablet,
          ]}
        >
          {categories.map((category) => (
            <View
              key={category}
              style={
                isTablet
                  ? styles.categoryTablet
                  : undefined
              }
            >
              <CategoryCard
                title={category}
                onPress={() =>
                  onOpenCategory(category)
                }
              />
            </View>
          ))}
        </View>

        <Pressable
          onPress={onOpenProfile}
          style={styles.profileButton}
        >
          <Text style={styles.profileButtonText}>
            👤 О приложении
          </Text>
        </Pressable>
      </ScrollView>
    </SafeAreaView>
  );
}

/* =====================================================
   ЭКРАН СПИСКА МЕСТ
===================================================== */

function PlacesScreen({
  category,
  onBack,
  onSelectPlace,
}) {
  const { places } = usePlaces();

  const { width } = useWindowDimensions();

  const isTablet = width >= 768;

  const filteredPlaces = places.filter(
    (place) =>
      place.category === category
  );

  return (
    <SafeAreaView style={styles.screen}>
      <ScrollView
        contentContainerStyle={[
          styles.container,
          isTablet && styles.containerTablet,
        ]}
      >
        <Pressable
          onPress={onBack}
          style={styles.backButton}
        >
          <Text style={styles.backText}>
            ← Назад
          </Text>
        </Pressable>

        <Text style={styles.screenTitle}>
          {category}
        </Text>

        <Text style={styles.screenSubtitle}>
          Найдено мест: {filteredPlaces.length}
        </Text>

        <View
          style={[
            styles.placesContainer,
            isTablet && styles.placesTablet,
          ]}
        >
          {filteredPlaces.map((place) => (
            <PlaceCard
              key={place.id}
              place={place}
              onPress={() =>
                onSelectPlace(place)
              }
            />
          ))}
        </View>
      </ScrollView>
    </SafeAreaView>
  );
}

/* =====================================================
   ЭКРАН ДЕТАЛЬНОЙ ИНФОРМАЦИИ
===================================================== */

function DetailsScreen({
  place,
  onBack,
}) {
  if (!place) {
    return (
      <SafeAreaView style={styles.screen}>
        <View style={styles.errorContainer}>
          <Text style={styles.errorText}>
            Место не найдено
          </Text>
        </View>
      </SafeAreaView>
    );
  }

  return (
    <SafeAreaView style={styles.screen}>
      <ScrollView>
        <Image
          source={{ uri: place.image }}
          style={styles.detailsImage}
        />

        <View style={styles.detailsContainer}>
          <Pressable
            onPress={onBack}
            style={styles.backButton}
          >
            <Text style={styles.backText}>
              ← Назад
            </Text>
          </Pressable>

          <Text style={styles.detailsCategory}>
            {place.category}
          </Text>

          <Text style={styles.detailsTitle}>
            {place.name}
          </Text>

          <View style={styles.detailsRating}>
            <Text style={styles.detailsStar}>
              ★
            </Text>

            <Text
              style={styles.detailsRatingNumber}
            >
              {place.rating}
            </Text>

            <Text style={styles.ratingLabel}>
              Рейтинг
            </Text>
          </View>

          <View style={styles.infoBox}>
            <Text style={styles.infoTitle}>
              📍 Адрес
            </Text>

            <Text style={styles.infoText}>
              {place.address}
            </Text>
          </View>

          <Text style={styles.descriptionTitle}>
            О месте
          </Text>

          <Text style={styles.description}>
            {place.description}
          </Text>
        </View>
      </ScrollView>
    </SafeAreaView>
  );
}

/* =====================================================
   ЭКРАН ПРОФИЛЯ
===================================================== */

function ProfileScreen({ onBack }) {
  return (
    <SafeAreaView style={styles.screen}>
      <View style={styles.profileContainer}>
        <Pressable
          onPress={onBack}
          style={styles.backButton}
        >
          <Text style={styles.backText}>
            ← Назад
          </Text>
        </Pressable>

        <View style={styles.avatar}>
          <Text style={styles.avatarText}>
            👤
          </Text>
        </View>

        <Text style={styles.profileTitle}>
          Путеводитель по Стамбулу
        </Text>

        <Text style={styles.profileSubtitle}>
          Мобильное приложение с рекомендациями
          интересных мест Стамбула.
        </Text>

        <View style={styles.profileCard}>
          <Text style={styles.profileCardTitle}>
            🇹🇷 Стамбул, Турция
          </Text>

          <Text style={styles.profileCardText}>
            В приложении представлены
            достопримечательности, парки и
            рестораны Стамбула.
          </Text>
        </View>

        <Text style={styles.version}>
          Практическая работа №4
        </Text>
      </View>
    </SafeAreaView>
  );
}

/* =====================================================
   ОСНОВНАЯ ЛОГИКА ПРИЛОЖЕНИЯ
===================================================== */

function AppContent() {
  const [screen, setScreen] =
    useState("categories");

  const {
    selectedCategory,
    selectedPlace,
    selectCategory,
    selectPlace,
  } = usePlaces();

  const openCategory = (category) => {
    selectCategory(category);
    setScreen("places");
  };

  const openPlace = (place) => {
    selectPlace(place);
    setScreen("details");
  };

  const goBackToCategories = () => {
    setScreen("categories");
  };

  const goBackToPlaces = () => {
    setScreen("places");
  };

  const openProfile = () => {
    setScreen("profile");
  };

  if (screen === "places") {
    return (
      <PlacesScreen
        category={selectedCategory}
        onBack={goBackToCategories}
        onSelectPlace={openPlace}
      />
    );
  }

  if (screen === "details") {
    return (
      <DetailsScreen
        place={selectedPlace}
        onBack={goBackToPlaces}
      />
    );
  }

  if (screen === "profile") {
    return (
      <ProfileScreen
        onBack={goBackToCategories}
      />
    );
  }

  return (
    <CategoriesScreen
      onOpenCategory={openCategory}
      onOpenProfile={openProfile}
    />
  );
}

/* =====================================================
   ЗАПУСК ПРИЛОЖЕНИЯ
===================================================== */

export default function App() {
  return (
    <PlacesProvider>
      <AppContent />
    </PlacesProvider>
  );
}

/* =====================================================
   СТИЛИ
===================================================== */

const styles = StyleSheet.create({
  screen: {
    flex: 1,
    backgroundColor: "#f5f7fb",
  },

  container: {
    padding: 20,
    paddingBottom: 40,
  },

  containerTablet: {
    width: "100%",
    maxWidth: 1100,
    alignSelf: "center",
  },

  header: {
    marginBottom: 30,
  },

  flag: {
    fontSize: 42,
    marginBottom: 8,
  },

  mainTitle: {
    fontSize: 36,
    fontWeight: "800",
    color: "#111827",
    marginBottom: 8,
  },

  mainSubtitle: {
    fontSize: 16,
    lineHeight: 24,
    color: "#6b7280",
  },

  sectionTitle: {
    fontSize: 23,
    fontWeight: "700",
    color: "#111827",
    marginBottom: 16,
  },

  categoriesContainer: {
    width: "100%",
  },

  categoriesTablet: {
    flexDirection: "row",
    flexWrap: "wrap",
    justifyContent: "space-between",
  },

  categoryTablet: {
    width: "48%",
  },

  categoryCard: {
    width: "100%",
    backgroundColor: "#ffffff",
    borderRadius: 18,
    padding: 20,
    marginBottom: 16,

    flexDirection: "row",
    alignItems: "center",

    shadowColor: "#000",
    shadowOffset: {
      width: 0,
      height: 3,
    },
    shadowOpacity: 0.08,
    shadowRadius: 8,

    elevation: 3,
  },

  categoryIcon: {
    width: 64,
    height: 64,
    borderRadius: 32,
    backgroundColor: "#eef2ff",

    alignItems: "center",
    justifyContent: "center",

    marginRight: 16,
  },

  categoryIconText: {
    fontSize: 30,
  },

  categoryContent: {
    flex: 1,
  },

  categoryTitle: {
    fontSize: 19,
    fontWeight: "700",
    color: "#111827",
    marginBottom: 6,
  },

  categorySubtitle: {
    fontSize: 14,
    color: "#6b7280",
  },

  profileButton: {
    alignSelf: "center",
    padding: 15,
    marginTop: 10,
  },

  profileButtonText: {
    color: "#4f46e5",
    fontSize: 15,
    fontWeight: "700",
  },

  pressed: {
    opacity: 0.7,
  },

  backButton: {
    marginBottom: 15,
    alignSelf: "flex-start",
  },

  backText: {
    color: "#4f46e5",
    fontSize: 16,
    fontWeight: "700",
  },

  screenTitle: {
    fontSize: 30,
    fontWeight: "800",
    color: "#111827",
    marginBottom: 6,
  },

  screenSubtitle: {
    fontSize: 15,
    color: "#6b7280",
    marginBottom: 20,
  },

  placesContainer: {
    width: "100%",
  },

  placesTablet: {
    flexDirection: "row",
    flexWrap: "wrap",
    justifyContent: "space-between",
  },

  placeCard: {
    width: "100%",
    backgroundColor: "#ffffff",
    borderRadius: 16,
    marginBottom: 16,
    overflow: "hidden",

    shadowColor: "#000",
    shadowOffset: {
      width: 0,
      height: 3,
    },
    shadowOpacity: 0.08,
    shadowRadius: 7,

    elevation: 3,
  },

  placeCardTablet: {
    width: "48%",
  },

  placeImage: {
    width: "100%",
    height: 190,
  },

  placeImageTablet: {
    height: 220,
  },

  placeContent: {
    padding: 15,
  },

  placeName: {
    fontSize: 19,
    fontWeight: "700",
    color: "#111827",
    marginBottom: 8,
  },

  placeAddress: {
    fontSize: 13,
    color: "#6b7280",
    lineHeight: 19,
    marginBottom: 10,
  },

  ratingRow: {
    flexDirection: "row",
    alignItems: "center",
  },

  star: {
    fontSize: 18,
    color: "#f59e0b",
    marginRight: 5,
  },

  rating: {
    fontSize: 14,
    fontWeight: "700",
    color: "#374151",
  },

  detailsImage: {
    width: "100%",
    height: 300,
    backgroundColor: "#e5e7eb",
  },

  detailsContainer: {
    padding: 20,
    paddingBottom: 40,
  },

  detailsCategory: {
    fontSize: 14,
    fontWeight: "700",
    color: "#4f46e5",
    marginBottom: 8,
  },

  detailsTitle: {
    fontSize: 32,
    fontWeight: "800",
    color: "#111827",
    marginBottom: 12,
  },

  detailsRating: {
    flexDirection: "row",
    alignItems: "center",
    marginBottom: 25,
  },

  detailsStar: {
    fontSize: 25,
    color: "#f59e0b",
    marginRight: 6,
  },

  detailsRatingNumber: {
    fontSize: 18,
    fontWeight: "700",
    color: "#111827",
  },

  ratingLabel: {
    fontSize: 14,
    color: "#6b7280",
    marginLeft: 7,
  },

  infoBox: {
    backgroundColor: "#eef2ff",
    padding: 17,
    borderRadius: 15,
    marginBottom: 28,
  },

  infoTitle: {
    fontSize: 16,
    fontWeight: "700",
    color: "#111827",
    marginBottom: 7,
  },

  infoText: {
    fontSize: 15,
    color: "#4b5563",
    lineHeight: 22,
  },

  descriptionTitle: {
    fontSize: 22,
    fontWeight: "700",
    color: "#111827",
    marginBottom: 10,
  },

  description: {
    fontSize: 16,
    color: "#4b5563",
    lineHeight: 26,
  },

  errorContainer: {
    flex: 1,
    alignItems: "center",
    justifyContent: "center",
  },

  errorText: {
    fontSize: 18,
    color: "#ef4444",
  },

  profileContainer: {
    flex: 1,
    padding: 20,
    alignItems: "center",
    justifyContent: "center",
  },

  avatar: {
    width: 90,
    height: 90,
    borderRadius: 45,

    backgroundColor: "#e0e7ff",

    alignItems: "center",
    justifyContent: "center",

    marginBottom: 20,
  },

  avatarText: {
    fontSize: 42,
  },

  profileTitle: {
    fontSize: 25,
    fontWeight: "800",
    color: "#111827",
    textAlign: "center",
    marginBottom: 10,
  },

  profileSubtitle: {
    fontSize: 15,
    lineHeight: 22,
    color: "#6b7280",
    textAlign: "center",
    marginBottom: 25,
  },

  profileCard: {
    width: "100%",
    maxWidth: 600,
    backgroundColor: "#ffffff",
    borderRadius: 18,
    padding: 20,

    shadowColor: "#000",
    shadowOffset: {
      width: 0,
      height: 3,
    },
    shadowOpacity: 0.06,
    shadowRadius: 8,

    elevation: 3,
  },

  profileCardTitle: {
    fontSize: 18,
    fontWeight: "700",
    color: "#111827",
    marginBottom: 10,
  },

  profileCardText: {
    fontSize: 15,
    lineHeight: 23,
    color: "#6b7280",
  },

  version: {
    marginTop: 25,
    fontSize: 13,
    color: "#9ca3af",
  },
});
